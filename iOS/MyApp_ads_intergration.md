# MyApp ads intergation

# Guide 

- AotterTrek iOS SDK contains trackiing and native advertising service.
- minion iOS version: 9.0
- MyApp ads support for SDK 3.1 and later.

# Install SDK

## Option 1. through Cocoapods

1. In .podfile

   ```
   //Podfile
   pod 'AotterTrek-iOS-SDK', '~> 3.1'
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


# initial SDK

initial MyApp Service in AppDelegate.m

```objective-c
// AppDelegate.m
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>


//only initial MyApp service
[[AotterTrek sharedAPI] initMyAppServiceWithClientId:@""
                                              secret:@""]; 
 
//initial MyApp service and TrekService if needed
[[AotterTrek sharedAPI] initTrekServiceWithClientId:@""
                                              secret:@""
                                       myAppClientId:@""
                                   myAppClientSecret:@""];
```



#usage 

Native ad `TKMyAppAdNative` usage is same as `TKAdNative`. See [Native Ad](iOS/Native_Ad?id=native-ad) for more information

```objective-c
  self.nativeAd = [[TKMyAppAdNative alloc] initWithPlace:@"myPlace" category:@"testCategory"];
  [self.nativeAd registerPresentingViewController:self];
  self.nativeAd.delegate = self;
  [self.nativeAd fetchAd];
```

# Overwrite click event for MyApp ads

implement `TKAdNativeDelegate`  's `TKMyAppAdNativeOnClicked:(TKMyAppAdNative *)ad`

```objective-c
-(void)TKMyAppAdNativeOnClicked:(TKMyAppAdNative *)ad{
    NSLog(@"[Demo MyApp] >> onClick MyAppAdNative with adData: %@", ad.AdData);
    NSString *targetUrl = ad.AdData[@"url_original"];
    //TODO: do click action for targetUrl or other manual field.
}
```

NOTE 1. This method is only avaiable for MyApp's ads.
NOTE 2. When implement this method, the default click event will be prevet, you should implement click action manullay such as open url / open browser.