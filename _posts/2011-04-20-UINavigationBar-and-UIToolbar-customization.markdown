---
layout: post
title:  "UINavigationBar and UIToolbar customization. Ultimate solution."
date:   2011-04-20 14:15:00
categories: UIKit
---

It was intended to be a big-big post about tricks and tips of UINavigationBar and
UIToolbar customization. Fortunately I’ve found an ultimate solution for these two
components which is actually two lines of code one of which is an import of a framework:

{% highlight objc %}
#import <QuartzCore/QuartzCore.h>
navigationBar.layer.contents = (id)backgroundImage.CGImage;
{% endhighlight %}