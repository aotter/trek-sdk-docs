# Native Ad

## 1. create adNative object

```objective-c
 //initial ad with place and category
   self.myAdNative = [TKAdNative alloc] initWithPlace:@"my_place" category:nil];

   //register current presenting view controller
   [self.myAdNative registerPresentingViewController:self];
   
   //set delegate if needed
   self.myAdNative.delegate = self;
   
   //fetch Ad data from API
   [self.myAdNative fetchAd];
```

## 2. render adNatvie UI object

```objective-c
-(void)TKAdNative:(TKAdNative *)ad didReceivedAdWithData:(NSDictionary *)adData{
    //register ad view.
    [self.myAdView registerAdView:self.myAdView];

    //render ad data
    NSDictionary *adData = ad.AdData;
       //{render views...}

   //se Action button
   [self.myAdNative registerCallToActionButton:self.myButton];
}
```

#### adData

| Variable       | Type   | description                            |
| -------------- | ------ | -------------------------------------- |
| adType         | String |                                        |
| uuid           | String |                                        |
| title          | String |                                        |
| sponser          | String |                                        |
| text           | String |                                        |
| advertiserName | String |                                        |
| img_icon       | String | 82x82                                  |
| img_icon_hd    | String | 300x300                                |
| img_main       | String | 1200x628                              |
| imgs           | Map    | custom images {label,src,width,height} |

## 3. ad fail delegates

```objective-c
-(void)TKAdNative:(TKAdNative *)ad fetchError:(TKAdError *)error{
    NSLog(@"TKAdNative fetch error: %@", error.message);
}
```

## 4. remove and release

```objective-c
//remove and release data
[self.myAdNative destroy];
```

## 5. more functions 

### isExpired
```objective-c
//check the ad is expired or not.
[self.myAdNative isExpired]; // YES or NO
```

### impression & click delegate
```objective-c
-(void)TKAdNativeWillLogClicked:(TKAdNative *)ad{
    //will log clicked
}

-(void)TKAdNativeWillLogImpression:(TKAdNative *)ad{
    //will log impression
}
```


