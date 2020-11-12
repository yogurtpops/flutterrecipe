---
layout: default
title: Play Asset Delivery in Flutter
parent: Messing with Flutter
---

Now Playstore make possible flexible asset delivery by provide us with Play Asset Delivery functionality. We can use this functionality by using app-bundle distribution and Play Core Library for managing the distribution. This library provide us with functions for checking assets availability, assets downloading, and obtaining assets absolute path location. 

Meanwhile you can't use Play Core Library directly with Flutter, so I make flutter_play_asset, a flutter package to access Play Core API for Play Asset Delivery functionality. This package implements the Play Core API in native part of the library in Kotlin, and use PlatformChannel to provide the function for managing the assets delivery to the flutter interface.

I make a demo to implement this functionality, an application that use a lot of assets. I use modularization to separate the app by it's functionalities, therefore I can easily devide the assets to a few parts. Then I decide which pack of assets which will be used later, and make the use case for user to obtains those assets later when they will actually need them. By doing so, I reduce the installation size by the size of the assets, in which for a few high res images each sized around 1 mb, it's total in 18 mb.

![Alt Text](https://merry-drylands.cloudvent.net/assets/images/play_asset_demo.gif)

[View on Github](https://github.com/yogurtpops/flutter_play_asset){: .btn .btn-blue .mr-2}
[Try It as Tester!](https://play.google.com/apps/internaltest/4699584195059121667){: .btn }

So far flutter_play_asset is limited to only support play asset delivery feature for downloading asset pack with on-demand mode, so there still a lot of improvement need to be done to make this library perfect. I'm very open to feedback and any contribution to this project will be very much appreciated!