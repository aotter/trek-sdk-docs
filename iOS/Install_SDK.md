# Guide 

- AotterTrek iOS SDK contains trackiing and native advertising service.
- minion iOS version: 9.0



# Install SDK

## Option 1. through Cocoapods

1. In .podfile
   ```
   //Podfile
   pod 'AotterTrek-iOS-SDK', '~> 3.0'
   ```
2. `pod install`

## Option 2.  manually
1. pull&drag AotterTrek-iOS-SDK to your project.
2. link AotterTrek-iOS-SDK if it's not auto-linked.
3. adding linked frameworks and librarires to your target
	- SystemConfiguration
	- CoreMedia
	- WebKit
	- CoreTelephony
	- AdSupport
	- AVKit
	- AVFoundation
	- Foundation
	- UIKit
4. install Google-IMA-iOS-SDK, reference: https://developers.google.com/interactive-media-ads/docs/sdks/ios/



# Initialzatiion & settings

```objective-c
// AppDelegate.m
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  //required
  [[AotterTrek sharedAPI] initTrekServiceWithClientId:@"<client id>"
                                               secret:@"<client secret>"];
  
  //optional
  [[AotterTrek sharedAPI] enableLoggerWithLevel:TKLoggerLevelDetail];

  
  //Optional, if you are implementing SuprAd and wish the ad video sound would not interrupt your users background music. 
  //If your app combines various audio playing conditions, and you are familiar to thes functions, it's ok to set any category that you want.
   [[AVAudioSession sharedInstance]setCategory:AVAudioSessionCategoryAmbient error:nil];
  
  
  
  return YES;
}
```

- test key
  - CLIENT_ID : `21tgwWwuzFYiD4ko5Klr`
  - CLIENT_SECRET : `fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44`

1. initService with your clientId and clientSecrete
2. enable logger if needed
3. TKLoggerLevel:
   1. `TKLoggerLevelNone`: no log will shown.
   2. `TKLoggerLevelNormal`: nessary logs or warning/error logs will shown.
   3. `TKLoggerLevelDetail`: all logs include verbose logs will shown.