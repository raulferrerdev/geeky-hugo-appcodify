---
title: "Flutter Forward 2023: Going Beyond"
description: "Learn where is heading Flutter."
date: 2023-01-26
categories: ["Flutter"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1hgZiqZid5nN2jG5Fgyuf-PbKA2ip8ubb"
draft: false
---

**Flutter** is a multi-platform software created by Google that allows us to develop mobile applications for both Android and iOS, desktop applications that run on Linux, Windows and macOs, and even for the web, always sharing the same code base.

Since its presentation a few years ago (2017), the acceptance of **Flutter** among developers has been increasing day by day, so that today we can find thousands of applications both in the Apple Store and in Google Play developed with **Flutter**.

Since its launch, the evolution of **Flutter** has been permanent and, yesterday, at **[Flutter Forward](https://flutter.dev/events/flutter-forward)**, the new features that are to be incorporated were presented.

{{< youtube zKQYGKAe5W8 >}}
Source: [Flutter](https://www.youtube.com/@flutterdev)

## Flutter 3.7 and Dart 2.19
Flutter 3.7 is the recent release that has added a lot of [features and enhancements](https://docs.flutter.dev/development/tools/sdk/release-notes), like:
* Material 3 support has improved
* [New graphical components are added](https://flutter.github.io/samples/web/material_3_demo/#/): Menu bars, context menus...
* New iOS features: iOS release validation, bitcode deprecation...
* Better rendering engine (Impeller)
* Improvements in memory management
Dart 2.19 has also some [news](https://dart.dev/guides/language/evolution#dart-219). One of the most significant is that simplifies concurrency with [Isolate.run()](https://medium.com/dartlang/better-isolate-management-with-isolate-run-547ef3d6459b).

## Improvements on Flutter development
In the Flutter Forward a series of important improvements were presented in which they are working to improve its use.

{{< image src="images/posts/flutter_forward_2023_1.png" alt="Flutter Forward improvements">}}

## Breakthrough graphics performance
Impeller, el motor de renderizado de Flutter, se ha reescrito en su mayor parte. De esta forma se ha mejorado de forma importante el rendimiento gráfico, a parte de corregir algunos errores preexistentes.
Inicialmente estas mejoras se están implementando para plataformas móviles (trabajando sobre la API de Android 3D Vulkan y sobre Metal en el caso de iOS). 

El siguiente video muestra la diferencia entre el motor antiguo y el nuevo.

{{< youtube Z7v-YRdHusM >}}
Source: [Tim Sneath](https://www.youtube.com/@timsneath2036)

## Seamless integration web and mobile
In the **Flutter Forward** improvements have been presented in the integration of Flutter at the web level and mobile applications.
### Web integration
A new feature has been introduced, called **element embedding**, which allows you to insert Flutter content on any website. With this, what is achieved is that our Flutter content becomes a Web component (forming part of the DOM), as can be seen in the [demo that was presented](https://flutter-forward-demos.web.app/#/).

### Mobile integration
Until now, to make use of native components of the iOS or Android platforms, the communication was done through messages and it was necessary to have knowledge of Kotlin (Android) or Swift (iOS) to complete the access to the native libraries used.
Now they are working on simplifying this whole process, and that Dart is in charge of automatically creating all these communications.

## Early to new and emerging architectures
Currently, we can develop applications with Flutter for Android, iOS, Linux, macOS, Windows and devices, and also for the web.
But progress has been made in Dart's support for new architectures, such as WebAssembly or RISC-V.

## Continued focus on developer experience
**Flutter Forward** has talk about how is focusing on developer experience:
* Improved [DevTools](https://docs.flutter.dev/development/tools/devtools/overview) is presented.
* Has introduced Dart 3, which is a major language update that introduces null sound security and removes deprecated features.
*  Alpha quality builds of Dart 3 and Flutter have been released for developers to test packages and apps.
*  The team is investing in developer expertise for Flutter, including the [News Toolkit](https://github.com/flutter/news_toolkit) to speed up mobile development for news publishers and other content providers. The News Toolkit includes everything you need to build an article-centric app, complete with navigation, search, authentication, ad integrations, notifications, profiles, and subscriptions.


## Conclusion
All this are great news for Flutter developers. If you would deep more in where is Flutter heading, you can check his [roadmap](https://github.com/flutter/flutter/wiki/Roadmap).