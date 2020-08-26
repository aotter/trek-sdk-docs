# Prepare For iOS 14

Starting in iOS 14, IDFA will be unavailable until an app calls the [App Tracking Transparency](https://developer.apple.com/documentation/apptrackingtransparency) framework to present the app-tracking authorization request to the end user. If an app does not present this request, the IDFA will automatically be zeroed out which may lead to a significant loss in ad revenue.

This guide outlines the changes needed for iOS 14 support.

# Request AppTrackingTransparency for IDFA

## Step 1. Link AppTrackingTransparency.framework in your project.

As the framework is iOS14+, you need to link this feature with the framework at beta Xcode (12+)

## Step 2. add User Tracking Usage Description in info.plist

Just like the usage of Camera / Album / Location , you need to provide a message for telling user that why your app need the permission.

```
<key>NSUserTrackingUsageDescription</key>
<string>This identifier will be used to deliver personalized ads to you.</string>
```

## Step 3. request permission before request ads.

To present the authorization request, call [`requestTrackingAuthorizationWithCompletionHandler:`](https://developer.apple.com/documentation/apptrackingtransparency/attrackingmanager/3547037-requesttrackingauthorization). We recommend waiting for the completion callback prior to loading ads, so that if the user grants the App Tracking Transparency permission, the Interactive Media Ads SDK can use the IDFA in ad requests.

```
#import <AppTrackingTransparency/AppTrackingTransparency.h>
#import <AdSupport/AdSupport.h>

- (void)requestIDFA {
  [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
    // Tracking authorization completed. Start loading ads here.
    // [self.myTKAdNative loadAd];
  }];
}
```

For more information about the possible status values, see [`ATTrackingManager.AuthorizationStatus`](https://developer.apple.com/documentation/apptrackingtransparency/attrackingmanager/authorizationstatus).



