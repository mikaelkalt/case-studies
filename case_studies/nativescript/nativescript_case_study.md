# NativeScript - A case study

In January 2018 we started a new mobile app project. The decision was made to use the [NativeScript](https://www.nativescript.org/) framework. One of the primary reasons for this decision was that our customer had a lot of experience in Angular development and would take over maintenance of the project once developed. Moreover our team was comprised of mostly former web application programmers well versed in Java and JavaScript and little experience with native mobile development technologies like Objective-C, Swift or the Android SDK.

This case study is a summary of our experience using NativeScript for mobile application development.

## No silver bullet

Firstly, it is very important to mention that despite the advertisment, using NativeScript successfully still requires that you have development experience in, or knowledge of, the underlying platform. Sooner or later you'll run into problems where you're happy to know what an *Intent* on Android is or what a *UINavigationController* on iOS is. In our case we had to write a plugin right from the beginging and we would have been lost if we had not had someone on the team with experience in native iOS mobile application development.

## Introduction

NativeScript is a Javascript-based framework for developing native mobile apps with full access to all native functionalities. JavaScript code is executed during runtime on the devices, there is no cross-compilation. For more information please visit [nativescript.org](https://www.nativescript.org/).

![nativescript-architecture](./images/ns-common.png "NativeScript Architecture")

To develop an app, one can choose between plain JavaScript, Angular or Vue.js. The framework provides core modules which abstract the basic functionalites of the underlying platforms. NativeScript also comes with a command-line interface (CLI) that lets you create, build and run the apps.

For our project we decided to use Angular since everyone on the team had worked with AngularJS and were in favour of the type safety provided by TypeScript.

## Tech stack

Since NativeScript provides a Javascript runtime, there are a lot of layers on top of the underlying platforms which brings extra complexity with it. Moreover, a framework like Angular brings its own complexity on top. In addition, it is not always clear which functionalites of Angular are fully supported by NativeScript.

To give you an idea of what's happening from the moment where you start building the app until you run it on your phone - you might see examples of errors like the following:

```sh
TODO?
```

For our project, we used webpack (which should become the default build-tool) to build our app. Thus webpack is responsible for compiling Typescript into Javascript, compiling the SCSS files (we were also using SASS) to CSS and finally bundling, minifying and uglifing everything. This is basically how a single page web application is built nowadays and comes with it's own complexity.

After the build process, the Javascript application is bundled into a native app. The NativeScript build process takes the bundled image of the project and completes the build with Native Build Tools (Xcode for iOS and Gradle for Android), which produces a native mobile app as a result.

Thus, there are many layers that can produce an error even during the build time.

### Error messages

As mentioned previously there are many layers in the NativeScript technology stack which can potentially cause problems. In our case, this problems often resulted in poor error messages which were hard to interpret and deduce the main cause of the problem. Since the introduction of webpack as build tool it got even harder as you can see in the example below:

```sh
TODO stacktrace
```

They're currenlty missing the line information.

## Plugins

When writing a NativeScript app you might reach a point where you ask yourself: "how can I implement a certain functionality which is not part of the NativeScript framework?", e.g. scan a QR code with the app. Fortunately, there are quite a few libraries for Android and iOS that support this functionality.

NativeScript comes with a [marketplace](https://market.nativescript.org/) that lists all of the installable plugins. They've even added a nice feature that gives you hints how well maintained a plugin is.

A plugin often just wraps an Android and iOS library and provides a common interface to it. This also brings some complexity into the game: How well maintained is the underlying library? So that means that if an iOS library needs to be updated for a new iOS version, how long does it take until the new version is available and how long does it take until the plugin is updated for that version? If you find a bug in the plugin you first have to make sure that it is the plugins fault and not that of the underlying library. If it is a problem of the underlying library it can take quite some time until the problem is fixed.

### Writing Plugins

Depending upon your requirements, it is very likely that you'll reach the point that you have to write your own plugin which, in our experience can be a real pain in the ass. Since we needed to integrate the [Scanbot SDK](https://scanbot.io/de/index.html), there was no way around writing a plugin. While the documentation of the Scanbot SDK is very good and there are a lot of examples on how to use their SDK with native code, we realized that the plugin is written in Javascript / Typescript which meant that we could not directly use the examples. The following is a plugin example with Android (Java), iOS (Objective-C) along with their translated JavaScript equivalents.

```java
TODO
```

```js
TODO
```

As can be seen, writing an Android plugin does not look so complicated. Let us now have a look at the Objective-C part of the plugin:

```objective-c

```

```js

```

* use typescript generator

* using objective-c protocols

Unfortunately we soon realized that we need to write another plugin but that's a different story then. (TODO: rewrite this sentence.)

## Support

With every technology decision you should make sure that there is enough information / support that could help you in case of a problem.

### Documentation

NativeScript [documentation](https://docs.nativescript.org/) provides most of the information on might need. Since NativeScript supports plain JavaScript, TypeScript, Angular or Vue.js it is sometimes hard to find the correct information you're looking for since the documentation isn't always very intuitively structured. When we started development, one could often find code samples using plain JavaScript in the Angular part of the documentation but that has since improved a lot.

We recommend starting with the tutorial if you're new to NativeScript. That's a very good way to get used to the technology stack and writing apps. While there also exists an integrated tutorial in the NativeScript Playground, we still recommend setting up the whole environment on your local machine and start coding there so that you can learn more about the NativeScript CLI.

Sometimes it felt that the documentation is rather high-level. Moreover, for plugin development, we were missing lots of information. But overall we would say that the documentation is quite helpful once you get used to the structure.

### Forum

In the beginning there was a NativeScript forum. We tried a few times to use it but it was very frustrating since there was always the same person answering your question by telling you that you should look at the documentation. We sometimes had the feeling that neither did the support staff not understood the question, nor did they ask for clarification.

At some point the forum closed down and our only available choice was to use [Stack Overflow](https://stackoverflow.com/questions/tagged/nativescript). In addition, there is a Slack Channel which might be helpful.

### It's open source

So the good news is that NativeScript is open source! Therefore, if you can't find a solution for your problem in the documentation or on Stack Overflow you can still have a look at the source code and analyze the problem yourself.

If you open a Pull Request on Github it usually doesn't take much time until it is merged. It seems the processes there are very efficient and they community is very thankful if you help out by fixing a bug.

On the other hand we noticed that it can take very long if after you've reported a bug until someone really cares about it - most likely since there are a lot of open bug reports.

## Conclusion

During project development, our level of frustration was often quite high, especially in the beginning when we had to develop two plugins. Given our requirements, we can say for sure that we would have been much faster if we had developed native applications for both Android and iOS.

For a long term perspective we have gained a lot of experience and we could improve our knowledge in the area of NativeScript. We now have a single codebase that might be easier to maintain - depending on upcoming changes.

In our opinion the NativeScript is a very good approach for a cross-platform framework, but it is still not yet as stable and easy to use as it thus, reducing productivity. Therefore, our recommendation for the moment would still be that you develop the app using the platform specific programming languages and frameworks.
