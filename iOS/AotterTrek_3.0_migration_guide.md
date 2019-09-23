# AotterTrek 3.x migration guide

## 1. SDK name changed

- SDK 1.x named `AotterService`
- SDK 3.x named `AotterTrek-iOS-SDK`
- import class change to `#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>`

## 2. Class prefix changed

- SDK 1.x: `AT`
  - e.g. `ATAdNative`,` ATAdVideo`,` ATAdManager`, `ATTracker`
- SDK 3.x: `TK`
  - e.g. `TKAdNative`, `TKAdMaganer`, `TKTracker`

## 3. Video ad and Interactive ad removed, replaced with new SuprAd

SDK 3.x removed video ad and interactive ad, but add new ad type `SuprAd` for Media ad playing.

|                |     1.x      |     3.x     |
| -------------- | :----------: | :---------: |
| Native ad      |  ATAdNative  | TKAdNative  |
| VideoAd        |  ATAdVideo   | *(removed)* |
| Interactive Ad | ATAdInteract | *(removed)* |
| Supr Ad        |      -       | TKAdSuprAd  |



## 4. XcodeColor Logs removed.

since Xcode 8 stop supporting third-party plug-ins, the [XcodeColors](https://github.com/robbiehanson/XcodeColors) is not avalible anymore. 

|                1.x                |         3.x         |
| :-------------------------------: | :-----------------: |
|         ATLoggerLevelNone         |  TKLoggerLevelNone  |
|        ATLoggerLevelNormal        | TKLoggerLevelNormal |
| ATLoggerLevelNormalWithXcodeColor |     *(removed)*     |
|        ATLoggerLevelDetail        | TKLoggerLevelDetail |
| ATLoggerLevelDetailWithXcodeColor |     *(removed)*     |



## 5. Ad cache setting changed.

- 1.x : `ATsetIndividualAdPoolSize:`
- 3.0, 3.1: `disableAdCachePool` and `enableAdCachePoolWithIndiviaulPoolSize:`
- 3.2~ : removed cache enable/disable setting, the cache will always on and cache system has been improved.

