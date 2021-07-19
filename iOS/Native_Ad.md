# Native Ad

## 1. Create Ad slot

Enter the slot management of [Application List](https://trek.aotter.net/publisher/list/app) , create slot name and type.

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

<img width="699" alt="nativead" src="https://user-images.githubusercontent.com/46350143/120260531-aa385b80-c2c8-11eb-82c1-43eea92c726f.png">



## 2. create adNative object

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
   //initial ad with place
   //Replace the value in initWithPlace to your own UUID
   self.myAdNative = [TKAdNative alloc] initWithPlace:@"YOUR UUID" category:nil];

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
  
    // Get ad data parameter
  	NSString *advertiserName = adData[@"advertiserName"];
		NSString *title = adData[@"title"];
		NSString *text = adData[@"text"];
  	NSString *imageIcon = adData[@"img_icon"];       //82x82
    NSString *imageIconHd = adData[@"img_icon_hd"];  //300x300
    NSString *imgMain = adData[@"img_main"];         //1200x628
    NSString *callToAction = adData[@"callToAction"];
    NSString *sponsor = adData[@"sponser"];
  
    //register ad view.
    [self.myAdView registerAdView:self.myAdView];

    //render ad data
    NSDictionary *adData = ad.AdData;
       //{render views...}

   //set Action button
   [self.myAdNative registerCallToActionButton:self.myButton];
}
```

#### adData

| Variable       | Type   | description      | Always have value  | Required to be displayed |
| -------------- | ------ | ---------------- | ------------------ | ------------------------ |
| advertiserName | String | Advertiser name  | Yes                | Recommended              |
| title          | String | Ad headline      | Yes                | Required                 |
| text           | String | Ad text          | No, could be empty | Recommended              |
| img_main       | String | Size:1200x628    | Yes                | Recommended              |
| img_icon       | String | Size:82x82       | Yes                | Recommended              |
| img_icon_hd    | String | Size:300x300     | Yes                | Recommended              |
| callToAction   | String | Ex: `"了解詳情"` | Yes                | Recommended              |
| sponser        | String | Sponsor          | Yes                | Required                 |



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

