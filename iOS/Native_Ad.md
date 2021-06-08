# Native Ad

## 1. Create Ad slot

Enter the slot management of [Application List](https://trek.aotter.net/publisher/list/app) , create slot name and type.

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

<img width="699" alt="nativead" src="https://user-images.githubusercontent.com/46350143/120260531-aa385b80-c2c8-11eb-82c1-43eea92c726f.png">



## 2. create adNative object

If you want to test, please use the test key & adPlace，see the below

- CLIENT_ID : `21tgwWwuzFYiD4ko5Klr`
- CLIENT_SECRET : `fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44`
- adPlace: `native`

```objective-c
 //initial ad with place and category
   self.myAdNative = [TKAdNative alloc] initWithPlace:@"native" category:nil];

   //register current presenting view controller
   [self.myAdNative registerPresentingViewController:self];
   
   //set delegate if needed
   self.myAdNative.delegate = self;
   
   //fetch Ad data from API
   [self.myAdNative fetchAd];
```

## 3. render adNatvie UI object

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

## 4. ad fail delegates

```objective-c
-(void)TKAdNative:(TKAdNative *)ad fetchError:(TKAdError *)error{
    NSLog(@"TKAdNative fetch error: %@", error.message);
}
```

## 5. remove and release

```objective-c
//remove and release data
[self.myAdNative destroy];
```

## 6. more functions 

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

