---
title: Overview
description: Cordova and PhoneGap
slug: /cordova
---

# Cordova and PhoneGap

Apache Cordova (and Adobe PhoneGap), created in 2008, is an open source project that enables web developers to use their HTML, CSS, and JavaScript content to create a native application for a variety of mobile and desktop platforms.

For more details on the history of Cordova and more details on how it works, [please see here](https://ionicframework.com/resources/articles/what-is-apache-cordova).

## Why was Capacitor created?

The open source space is filled with new projects that build on top of the ideas of older projects, making tangible improvements that can't be done without radically changing the original product. The Ionic team didn't want to try to force these radical changes into Cordova for technical and political reasons.

One benefit of the Capacitor project is that the Ionic team has more control over the stack. When you build an app with Ionic Framework and Capacitor, the Ionic team are the maintainers of the native runtime layer, the UI components, and the toolchain that created the components ([Stencil](https://stenciljs.com/)). This is significant because the Ionic team can make fixes much quicker and offer a much more cohesive stack.

## Differences between Capacitor and Cordova

In spirit, Capacitor and Cordova are very similar. Both manage a Web View and provide a structured way of exposing native functionality to your web code. However, Capacitor has a few key differences that require web developers, previously used to Cordova's approach, to change app development workflows.

### Native Project Management

Capacitor considers each platform project a _source asset_ instead of a _build time asset_. That means you'll check your Xcode and Android Studio projects into source control, as well as use those IDEs when necessary for platform-specific configuration and building/testing.

This change in approach has a few implications. First, Capacitor does not use `config.xml` or a similar custom configuration for platform settings. Instead, configuration changes are made by editing the appropriate platform-specific configuration files directly, such as `AndroidManifest.xml` for Android and `Info.plist` for iOS. Capacitor does have some [high level configuration options](/docs/basics/configuring-your-app). These generally don't modify native functionality, but control Capacitor's tooling.

Additionally, Capacitor does not offer a way to build native apps on the command line. Platform-specific tooling (or in the IDE) should be used instead, which provides a faster, more typical experience that follows the standards of app development for that platform.

While these differences may be concerning to long-time Cordova users, there are worthwhile benefits:

1. Updating and modifying native projects through abstracted-away tools such as `config.xml` is error prone and a constant moving target. Becoming more comfortable with platform-specific tooling makes troubleshooting issues that much easier.
2. It's easier to add custom native code that your app needs without having to build a dedicated plugin for it outside of your app's codebase. Additionally, native teams can work alongside web teams on the same project.
3. Creating more compelling app experiences is now easier since you "own" the native project, such as adding a native UI shell around your web app.
4. More visibility into native project changes and better app maintainability as new mobile operating system versions are released. When breaking changes to Capacitor are introduced or changes are applied to the native project templates, the team will publish step-by-step upgrade instructions to ensure that the update process is as smooth as possible.

### Plugin Management

Capacitor manages plugins in a different way than Cordova. First, Capacitor does not copy plugin source code to your app before building. Instead, all plugins are built as "frameworks" (on iOS) and "libraries" (on Android) and installed using the leading dependency management tool for each platform (CocoaPods and Gradle, respectively). Additionally, Capacitor does not modify native source code, so any necessary native project settings must be added manually (for example, permissions in `AndroidManifest.xml`). We think this approach is less error-prone and makes it easier for developers to find help in the community for each specific platform.

One major difference is the way plugins handle the JavaScript code they need in order to be executed from the WebView. Cordova requires plugins to ship their own JavaScript and manually call `exec()`. Capacitor, in contrast, registers and exports all JavaScript for each plugin based on the native methods it detects at runtime, so all plugin methods are available as soon as the WebView loads. One important implication of this: there is no more need for the `deviceready` event. As soon as your app code loads, you can start calling plugin methods.

While Capacitor doesn't require plugins to provide JavaScript for iOS or Android, it is common for plugins to have shared logic in JavaScript, which is also easy to accomplish.

Finally, Capacitor has implications for plugin authors. On iOS, Swift 5 is officially supported and even _preferred_ for building plugins (Objective-C is also supported). Plugins no longer export a `Plugin.xml` file; Capacitor provides a few simple macros on iOS and annotations on Android for adding metadata to your plugin source code that Capacitor reads at runtime.

### CLI/Version Management

Capacitor, unlike Cordova, does not use a global CLI. Instead, the Capacitor CLI is installed locally into each project as an npm script. This makes it easier to manage versions of Capacitor across many different apps.

Thus, instead of running directly from the command line, Capacitor is invoked by calling `npx cap` in the directory of your app.

[Learn more about the Capacitor CLI &#8250;](/docs/cli)

## Start the Migration

Learn more about the [migration process](/docs/cordova/migration-strategy) or [get started migrating](/docs/cordova/migrating-from-cordova-to-capacitor) right away.
