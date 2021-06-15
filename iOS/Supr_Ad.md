# Supr Ad

## 1. Create Ad slot

Enter the slot management of [Application List](https://trek.aotter.net/publisher/list/app) , create slot name and type.

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

<img width="695" alt="suprad" src="https://user-images.githubusercontent.com/46350143/120260535-aad0f200-c2c8-11eb-861d-b17cc9b1b1d6.png">



## 2. create SuprAd object

If you want to test, please use the test key & adPlace，see the below

- CLIENT_ID : `21tgwWwuzFYiD4ko5Klr`
- CLIENT_SECRET : `fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44`
- Ad Place UUID: "YOUR ADPLACE UUID"

##### Note:

For the publisher who use AotterTrek SDK for the first time and didn't get the full access of ad slot management, please contact Aseal representative or [E-mail to us](https://aseal.in/contactus).

Please **use your own** Client ID and Secret as well as UUID for production environment. Once you finishing this setting, you can switch **production / test mode** by changing **your client id and test client id.**

- CLIENT_ID : "YOUR CLIENT ID"
- CLIENT_SECRET : "YOUR CLIENT SECRET "
- Ad Place UUID: "YOUR ADPLACE UUID"

```objective-c
// Replace the value in initWithPlace to your own UUID
self.suprAd = [[TKAdSuprAd alloc] initWithPlace:@"YOUR UUID" category:@""];
self.suprAd.delegate = self;

//register view controller for presenting
[self.suprAd registerPresentingViewController:self];
```



## 3. fetch ad
```objective-c
[self.suprAd fetchAd];
```



## 4. received Ad datat with prefered ad size, register for adView and TKMediaView

```objective-c
-(void)TKAdSuprAd:(TKAdSuprAd *)suprAd didReceivedAdWithAdData:(NSDictionary *)adData preferedMediaViewSize:(CGSize)size isVideoAd{
	if(isVideoAd){
   		//when this suprAd is video type.
	} 

	//TKMediaView: the view for showing SuprAd's Media
  //It is recommended to create the size of the view based on ad size.
  // EX: 「  If someone plan to use Screen width as Ad view width, "preferedMediaViewSize" is very recommended to calculate the corresponding Ad view height. 」
  
  // double adSizeWidth = preferedAdSize.width;
  // double adSizeHeight = preferedAdSize.height;      
  // CGFloat viewWidth = UIScreen.mainScreen.bounds.size.width;
  // CGFloat viewHeight = viewWidth * adSizeHeight/adSizeWidth;
  // int adheight = (int)viewHeight; // This result is the ad height I wanted.
  

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
| img_main       | String | 1200x628                               |
| imgs           | Map    | custom images {label,src,width,height} |

 

## 5. Ad load completed, the ad is ready to show on screen

```objective-c
-(void)TKAdSuprAdCompleted:(TKAdSuprAd *)suprAd{
		[self.adCell setNeedsLayout];
		[self.mainTableView reloadData];
}
```

## 6. Any error callback

```objective-c
-(void)TKAdSuprAd:(TKAdSuprAd *)suprAd adError:(TKAdError *)error{
		NSLog(@"SuprAd ad error: %@", error.description);
}
```

## 7. Notify While Ad Scrolled

if you render the ad in your scrollView/TableView/CollectionView or anything may have vertical scroll behavior, please call this fuction when the scrollView scrolled.

such as -(void)scrollViewDidScroll:(UIScrollView *)scrollView;

```objective-c
//example.
-(void)scrollViewDidScroll:(UIScrollView *)scrollView{
    if(self.suprAd){
        [self.suprAd notifyAdScrolled];
    }
}
```



## 8. Destroy ad 

```objective-c
-(void)viewDidDisappear:(BOOL)animated{
		[super viewDidDisappear:animated];
  
		//destroy the ad completedly.
		//while TKAdSuprAd manage itself, destroy() is not necessary. But It's nice to have it when you pretty sure the view/view controller is not useds anymore.
		[self.suprAd destroy];
}
```

## 9. more functions 

### isExpired
```objective-c
//check the ad is expired or not.
[self.suprAd isExpired]; // YES or NO
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



## Note :  

- Supr.Ad including video ad advertising, which is using VAST technology provided by the Google IMA SDK. In the implementation of VAST, Trek SDK will need to register ViewController when request VAST ads from Google IMA. This ViewController should be the same ViewController that displays the video ads!

  EX:

  A viewController requests ads, A viewController displays ads (o)

  A viewController requests ads, B viewController displays ads (x)

