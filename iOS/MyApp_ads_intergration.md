# MyApp ads intergation



## 1. support

1. SDK version greater than `3.1.x`
2. Installation via cocoapods
   1. pod 'AotterTrek-iOS-SDK', '~> 3.1'



## 2. initial

check the MyApp service is initialed.

```objective-c
//only initail Trek service
[[AotterTrek sharedAPI] initTrekServiceWithClientId:@""
                                             secret:@""];
 
//only initial MyApp service
[[AotterTrek sharedAPI] initMyAppServiceWithClientId:@""
                                              secret:@""]; 
 
//initial both Trek Service + MyApp Service
[[AotterTrek sharedAPI] initTrekServiceWithClientId:@""
                                              secret:@""
                                       myAppClientId:@""
                                   myAppClientSecret:@""];
```



## 3. usage 

Native ad `TKMyAppAdNative` usage is same as `TKAdNative`. See [guide](iOS/Native_Ad?id=native-ad) for more information

```objective-c
  self.nativeAd = [[TKMyAppAdNative alloc] initWithPlace:@"myPlace" category:@"testCategory"];
  [self.nativeAd registerPresentingViewController:self];
  self.nativeAd.delegate = self;
  [self.nativeAd fetchAd];
```



## 4. overwrite click event for MyApp ads

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