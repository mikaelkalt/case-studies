# NativeScript - A case study

In January 2018 we started a new mobile app project. The decision was made to use the quite new Framework [NativeScript](https://www.nativescript.org/). One reason for this decision was that our customer has a lot of experience in Angular development and our team was mainly built of Java and web developers with JavaScript knowledge and lack of mobile development knowledge like Objective-C, Swift or Android. 

During this project we were facing lots of challenges and I'd like to share our practical experience so it might help you in your next technology decision. 

## Don't be naiv

First of all I think it is important to mention that it would be very naiv to think that frameworks like NativeScript would prevent you from needing any knowledge of the underlying platforms. The sooner or later you'll run into problems where you're happy to know what e.g. an *Intent* on Android is or what a *UINavigationController* on iOS is. In our case we had to write a plugin right from the beginnging and I'd would have been lost without my expericence in iOS development. 

## Introduction

For those who don't know yet NativeScript that well, I'll give a short introduction. For more information please visit their [website](https://www.nativescript.org/). 

So as the name already points out, we're really creating native apps with full access to all native functionalities. Basically the JavaScript code is executed during runtime on the devices, there is no cross-compilation. 

![nativescript-architecture](./images/ns-common.png "NativeScript Architecture")

Basically one can choose between plain JavaScript, Angular or Vue.js to build your app. The framework provides the core modules which abstracts the basic functionalites of the underlying platforms. 

There is a command-line interface (CLI) that lets you create, build and run the apps.

For our project we decided to use Angular as framework on top. We were all experienced in AngularJS and we're all quite experienced in Java development that's why we'd prefere to use TypeScript.

## Tech stack complexity

As the NativeScript architecture already suggests, there are a lot of layers on top of the underlying platforms which brings an extra complexity into the game.

Since we're using Angular - we have another complexity on top. Angular is not as lightweight as one would hope and can get quite complex as well. In addition, it is not always clear which functionalites of Angular are really supported by NativeScript. 

To give you an idea of what's happening from the moment where you start building the app until you run it on your phone - you might see how many possible error source appear on this way: 

We're using webpack (which should become the default build-tool, as much as I'm aware) to build the app. So webpack is responsible for compiling the TypeScript into JavaScript, compiling the SCSS files (we're using SASS) to CSS and bundling, minifying and uglyfing everything. This is basically a normal web app build nowadays and you might now that this can get complex as well. 

On top of that the bundled JavaScript application is bundled into a native app. The NativeScript build process takes the bundled image of the project and completes the build with Native Build Tools (Xcode for iOS and Gradle for Android), and as a result produces a native mobile app.

So there are many layers that can produce an error even during the build time.

### Error messages 
 
As mentioned there are many layers of possible errors which in the end often result in poor error messages they are hard to interpret and figure out the main cause of the problem. Since the introduction of webpack as build tool it got even harder as you can see in the example below: 

```
TODO stacktrace
```

They're currenlty missing the line information.

## The plugin hell

When writing an app you'll reach the point where you're asking yourself how can I implement a certain functionality which is not part of the NativeScript framework, like e.g. scan a QR code with the app, but you might know that there are libraries for Android and iOS that support this functionality. 

### Pre-Builts Plugins

Luckily there are a lot of plugins for NativeScript that have already implemented such functionalites. 

There is a [marketplace](https://market.nativescript.org/) that lists all of the plugins. They've added even a nice feature that gives you hints how well maintained a plugin is.

A plugin often just wraps an Android and iOS library and provides a common interface to it. Which also brings some complexity into the game. How well maintained is the underlying library? So that means if e.g. an iOS library needs to be updated for a new iOS version - how long does it take until the new version is available and how long does it take until the plugin is updated for that version. If you find a bug in the plugin you first have to make sure that it is the plugins fault and not the underlying library. If it is a problem of the underlying library it can take quite some time until the problem is fixed. 

### Writing Plugins

Unfortunately it is very likely that you'll reach the point that you have to write your own plugin which can be a real pain in the ass. Since we needed to integrate the [Scanbot SDK](https://scanbot.io/de/index.html) there was no way around writing a plugin. So the documentation of the Scanbot SDK is very good and there are a lot of examples how to use their SDK that made use promising that it shouldn't be that hard to write the plugin. But then we've realized that the plugin is written in JavaScript / TypeScript and not in the platform specific language which kind of made the examples halfway useless. To show you an example what that means I've prepared two code snipped for Android and iOS in the original languages Java and Objective-C and then the translated JavaScript equivalent. 

Let's start with the easier platform. Since Java and JavaScript is not as different as Objective-C and JavaScript, we start with the easier example: 

Describe 

```java

```

```javascript

```

I've found it a bit weird but it is feasible to rewrite it pretty fast. BUT now let's have a look at the Objective-C part of the plugin. Since Objective-C is probably not as famous as Java I'd like to mention that one really needs to get used to the syntax which makes writing Objective-C code in JavaScript very hard. 

```objective-c

```

```javascript

```

* use typescript generator

* using objective-c protocols

Unfortunately we soon realized that we need to write another plugin but that's a different story then. 

## Support 

With ever technology decision you should make sure that there is enough information / support that could help you in case of a problem.

### Documentation
 
The [documentation](https://docs.nativescript.org/) provides most information you need. Since NativeScript supports plain JavaScript/TypeScript, Angular or Vue.js it is sometimes hard to find the correct information you're looking for since the documentation isn't always very intuitively structured. In the beginning one could often find code sample using plain JavaScript in the angular part of the documentation but that improved a lot. I think they're still looking for the best way to structure to documentation since it recently changed a few times. 

I'd recommend to start with the tutorial if you're new to NativeScript. That's a very good way to get used to the technology stack and the way to write apps. They've recently integrated the tutorial in the new NativeScript Playground but I'd still recommend to setup the whole environment on your local machine and start coding there so you'll learn more about the NativeScript CLI. 

Sometimes it felt that the documentation is quite high-level and especially in the plugin development I was missing lots of information. But overall I'd say the documentation is quite helpful if you're used to the structure and learned where to find the correct information. 

### Forum 

In the beginning there was an own NativeScript forum. I've tried a few times to use it but it was very frustrating since there was always the same person answering your question by telling you that you should look at the documentation even though I had the feeling he didn't even understood the question.

At some point they've decided to close the forum and only use [Stack Overflow](https://stackoverflow.com/questions/tagged/nativescript). In addition, there is a Slack Channel which might be helpful.

### It's open source

So the good news is that NativeScript is open source! So if you can't find a solution for your problem in the documentation or on Stack Overflow you can still have a look at the source code and analyze the problem yourself. 

If you open a Pull Request on Github it usually doesn't take much time until it is merged. It seems the processes there are very efficient and they community is very thankful if you help out by fixing a bug. 

On the other hand I've noticed that usually it can take very long if you're reporting a bug until someone really cares about it - most likely since there are a lot of open bug reports.

## Conclusion

During the project the level of frustration was often quite high, especially in the beginning when we had to develop the two plugins. I can say for sure that we would have been much faster if we'd developed twice a native app, without any doubts. 

On a long term perspective we have gained a lot of experience and we could improve our knowledge in the area of NativeScript. We have now a single code base that might be easier to maintain - depending on the upcoming changes. 

In my opinion the NativeScript is a very good approach for a cross-platform framework, but it is still not yet as stable and easy to use as it must be to be very productive. So therefore my personal recommendation for the moment would still be that you develop the app using the platform specific programming languages and frameworks.