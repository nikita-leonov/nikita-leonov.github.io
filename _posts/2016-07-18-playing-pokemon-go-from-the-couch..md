---
layout: post
title: "Playing Pokemon GO from the couch."
description: "Not every player knows, but it is possible to play Pokemon GO without going out into real world. Not everyone knows this cheat, so I want to be sure that it is available for the masses."
comments: true
keywords: "fun, pokemon go, cheat, hack, ios"
---

**Disclaimer:** This point going to be very controversial. It is about cheating in everyone’s beloved game Pokemon GO. Cheating is bad and many of us invested a lot of time in running around and catching rare pokemons, that is why I found it unfair that it is possible to cheat in pokemons and knowledge how to cheat is limited to a very small groups of people. I believe it should be shared, so no one will have an advantage in compare with others. Hopefully my good intention will be understood by the audience and I would not be blamed for this post.
The nature of a cheat is simple — It allows you to fake your current location on the phone, so you can catch pokemons whenever you are. Proposed way of cheating works only for iPhones & iPads, as it depends on special set of tools that available for iOS developers. While description may looks  a little bit technical, the goal is to describe it step by step, so everyone will be able to use it. Let me know in comments below if something is unclear, so I will adjust article further.

### Step #1. Downloading tools.
1. You will need download and install Xcode, that is available here — http://developer.apple.com/ios/. You will need an Apple ID to login if you do not have one, you can create a new one right there. 
  * ![tools-download](http://leonov.co/assets/images/2016/07/pokemon-go/1-tools-download.png) 
2. You will need to download a placeholder app (link) that you will need to download and run in Xcode on your phone. An app is absolutely empty and contains no logic, the only purpose is to allow starting fake locations to the phone.

### Step #2. Testing setup.
1. Open a placeholder app that we downloaded on step 1.2. by double-click GoTrainer.xcworkspace. Xcode should pop-up. Follow any asks or recommendations that Xcode will bring, as you lunching it first time it may ask you to accept some Terms of Service or download additional modules.
2. Connect your phone with a cable to your device.
3. Open devices management by pressing Window > Devices and in list of devices find device that  you just connected and press “use for development”. (Note: I do not have device that is not available for development, so can not provide an exact screenshot, you will need to figure it out own your own.)
  * ![device-management](http://leonov.co/assets/images/2016/07/pokemon-go/2-device-management.png)
4. Come back to the main window and select your phone from dropdown. Press build & run. Give it some time to run the app on the phone.
  * ![run-app](http://leonov.co/assets/images/2016/07/pokemon-go/3-run-app.png)
5. At this point you should be able to see instructions on the screen of iPhone, so go ahead and select desired route from a a Xcode dropdown.
  * ![select-route](http://leonov.co/assets/images/2016/07/pokemon-go/4-select-route.png)
6. Minimize an app, but keep your phone connected and Xcode on. Run Pokemon Go and verify that your trainer is in the place of the route and walking by itself. Enjoy 😃
7. If you want to change a route or location you can return back to Xcode and repeat step 2.5.

### Step #3. Modifying your routes.
1. Duplicate one of existing routes from Route folder in the list by selecting it and pressing File > Duplicate.
  * ![duplicate-route](http://leonov.co/assets/images/2016/07/pokemon-go/5-duplicate-route.png)
2. Go to Google Maps (http://maps.google.com) and collect coordinates of desired route points by just clicking points that you need.
  * ![google-map-coordinates](http://leonov.co/assets/images/2016/07/pokemon-go/6-google-map-coordinates.png)
3. Modify content of recently duplicated .gpx file by replacing coordinates with those you just collected in 3.2. Add any additional points that you need but increase time each time between points. Time attribute will be used to define speed of walk for you trainer, so do not define it too low or too big to pretend like you are walking.
  * ![modify-route](http://leonov.co/assets/images/2016/07/pokemon-go/7-modify-route.png)
4. Ensure that the last coordinate of the route is the same as first coordinate of the route, only in such case you will get smooth walking in rounds and will not be watched by anti-cheating system.

This is it. By now you should be able to catch (`)em all no matter what part of the world they are. I recommend to be accurate with this cheat and do not change location really rapidly, since your account can be blocked for some time from accessing items from PokeStops. If you want to appear somewhere, it is better to define a route from the place where you are to the place where you want to be and ensure that speed of travel is reasonable. I am sure that eventually even this way of cheating will be prevented by analysis, but you will just need to define more random / zig-zag ways of travel so they will look more natural to workaround this. Good luck!
