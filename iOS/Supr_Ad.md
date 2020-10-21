# Supr Ad

## 1. create SuprAd object
```objective-c
self.suprAd = [[TKAdSuprAd alloc] initWithPlace:@"somewhere" category:@""];
self.suprAd.delegate = self;

//register view controller for presenting
[self.suprAd registerPresentingViewController:self];
```



## 2. fetch ad
```objective-c
[self.suprAd fetchAd];
```



## 3. received Ad datat with prefered ad size, register for adView and TKMediaView

```objective-c
-(void)TKAdSuprAd:(TKAdSuprAd *)suprAd didReceivedAdWithAdData:(NSDictionary *)adData preferedMediaViewSize:(CGSize)size isVideoAd{
if(isVideoAd){
   //when this suprAd is video type.
} 

//TKMediaView: the view for showing SuprAd's Media
[self.suprAd registerTKMediaView:self.adCell.contentView];

//AdView: the container view for ad
[self.suprAd registerAdView:self.adCell.contentView];

//prefered size for the ad
self.myAdSize = size;

NSString *title = adData[kTKAdTitleKey];
NSString *contextText = adData[kTKAdTextKey];
NSDictionary *images = adData[kTKAdImgsKey];
}
```

```
WARNING: if this suprad is video type, you should registerTKMediaView and presentingViewController before the end of 'didReceivedAdWithData' delegate.
if you want to regsiter TKMediaView & presentingViewController just before you show on view, you could call 
loadAd' right after regsiters, but it would take some time to show vidoe.
```


### adData

| Variable       | Type   | description                            |
| -------------- | ------ | -------------------------------------- |
| adType         | String |                                        |
| uuid           | String |                                        |
| title          | String |                                        |
| text           | String |                                        |
| advertiserName | String |                                        |
| img_icon       | String | 82x82                                  |
| img_icon_hd    | String | 300x300                                |
| img_main       | String | 1200x627                               |
| imgs           | Map    | custom images {label,src,width,height} |

 

## 4. Ad load completed, the ad is ready to show on screen

```objective-c
-(void)TKAdSuprAdCompleted:(TKAdSuprAd *)suprAd{
[self.adCell setNeedsLayout];
[self.mainTableView reloadData];
}
```

## 5. Any error callback

```objective-c
-(void)TKAdSuprAd:(TKAdSuprAd *)suprAd adError:(TKAdError *)error{
NSLog(@"SuprAd ad error: %@", error.description);
}
```

## 6. Destroy ad 

```objective-c
-(void)viewDidDisappear:(BOOL)animated{
[super viewDidDisappear:animated];
//destroy the ad completedly.
//while TKAdSuprAd manage itself, destroy() is not necessary. But It's nice to have it when you pretty sure the view/view controller is not useds anymore.
[self.suprAd destroy];
}
```


## 7. more functions 

### isExpired
```objective-c
//check the ad is expired or not.
[self.suprAd  isExpired]; // YES or NO
```

### isVideoAd
```objective-c
   [self.suprAd isVideoAd]; //YES or NO
```

### impression & click delegate
note. unavaiable for video type supr ad 
```objective-c
-(void)TKAdSuprAdWillLogImpression:(TKAdSuprAd *)ad{
    NSLog(@"TKSuprAd log impression");
}

-(void)TKAdSuprAdWillLogClick:(TKAdSuprAd *)ad{
    NSLog(@"TKSuprAd log click");
}
```



