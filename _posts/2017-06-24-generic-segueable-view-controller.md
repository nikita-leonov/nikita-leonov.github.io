---
layout: post
title: "Generic Segueable View Controller."
description: "UIKit does not leverage Swift generics which makes use of generics with UIKit classes very not intuitional. I wrote a code sample that shows a trick that allows using generics with UIKit without limitations."
comments: true
keywords: "ios, swift, generics, uikit, view controller, mvc"
---

Let’s apply the power of generics to UIViewController. The goal will be to generalize a controller with an object that conforms to a base empty protocol ModelProtocol and is able to perform UIStoryboardSegues without stripping type inform.  Here is our starting point:
```swift
public protocol ModelProtocol {}

open class ViewController<Model: ModelProtocol>: UIViewController {
    public var model: Model!
}
```

By Apple's design `prepare(for:UIStoryboardSegue, sender:Any?)` is the place to typecast a segue's source and destination controllers to a concrete type and perform necessary data transformations. Let’s imagine we have `ViewControllerA<ModelA>` that segues to `ViewControllerB<ModelB>` and we want on a segue to create `ModelB` from a data stored in `ModelA`. As all the segues are known, we can do an explicit typecasting including models types. Our prepare for a segue will look like following:
```swift
public override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let source = segue.source as? ViewControllerA<ModelA>, let destination = segue.destination as? ViewControllerB<ModelB> else {
        return
    }
    //TODO: Data transformation
}
```

Unfortunately, it is not so easy. At this point compiler will provide following errors:
![specialize-non-generic-type-error](http://leonov.co/assets/images/2017/06/generic-segueable-view-controller/specialize-non-generic-type-error.png)

As we found Swift does not allow to cast non-generic classes to generic ones, luckily there is a solution to such problem. It is correct to say, that every object knows its own type. So, the only thing left is to call through each object one by one and ensure that the final object in a call chain will dispatch all information about types into a generic prepare for a segue function. Let’s also enforce some directionality of a data flow in our architecture, by introducing following interfaces that our ViewController will conform:
```swift
public protocol ModelContainerProtocol {
    associatedtype Model: ModelProtocol
    var model: Model! { get }
}

public protocol MutableModelContainerProtocol: ModelContainerProtocol {
    var model: Model! { get set }
}
```

Here is a generic prepare for a segue function:
```swift
open func prepare<Source, Destination>(segueId: String, source: Source, destination: Destination) where Source: ModelContainerProtocol, Destination: MutableModelContainerProtocol {}
```

The interface for gluing together generic and non-generic protocols will look as follows:
```swift
public protocol AnySegueable {
    func prepare<Source, Destination>(segueId: String, source: Source, destination: Destination) where Source: ModelContainerProtocol, Destination: MutableModelContainerProtocol
    func dispatchPrepare(segueId: String, destination: AnySegueable)
    func dispatchPrepare<Other>(segueId: String, source: Other) where Other: AnySegueable & ModelContainerProtocol
}
```

There are two `dispatchPrepare` functions that should be called on an instance. Self type will be known within a body of each function. If we chain them together properly we will be able to restore all necessary types information. We will use protocol extension to implement that logic:
```swift
public protocol Segueable: AnySegueable, MutableModelContainerProtocol {}

extension Segueable {
    public func dispatchPrepare(segueId: String, destination: AnySegueable) {
        destination.dispatchPrepare(segueId: segueId, source: self)
    }

    public func dispatchPrepare<Other>(segueId: String, source: Other) where Other: AnySegueable & ModelContainerProtocol {
        source.prepare(segueId: segueId, source: source, destination: self)
    }
}
```

A final accord will be a properly rewritten implementation of a generic segueable view controller:
```swift
open class ViewController<Model: ModelProtocol>: UIViewController, Segueable {
    public var model: Model!

    open func prepare<Source, Destination>(segueId: String, source: Source, destination: Destination) where Source: ModelContainerProtocol, Destination: MutableModelContainerProtocol {}

    override final func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        guard let segueId = segue.identifier, let source = segue.source as? AnySegueable, let destination = segue.destination as? AnySegueable else {
            return
        }
        source.dispatchPrepare(segueId: segueId, destination: destination)
    }
}
```

A default prepare for a segue function is annotated as final to enforce the use of its generic counterpart. The workflow is the same as with an original function. We can downcast source & destination to generic types within new generic prepare for a segue.

Enabling generics for segue transformations not only allows to formalize an architecture but also opens a possibility for an implementation of other elegant solutions such as routers. In a further article, I am going to show one of such solutions.

Code of the solution, together with a basic use-case sample is available on GitHub - [http://github.com/nikita-leonov/GenericMVC].

**Credits**: The major part of a credit for this solution goes to Dave Abrahams as he was the one helped me on the WWDC 2016 Lab to find such an elegant way to restore typing information.

**Note**: Code is Swift 4 only. It is possible to implement a similar solution on Swift 3. However, it will be less elegant because of the way how ObjC & Swift runtimes register generics and how IB discovers classes inherited from generalized base classes. Feel free to get in touch if you are interested to know more.
