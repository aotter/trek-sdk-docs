# Banner Ad

## 1. Create Ad slot

Enter the slot management of [Application List](https://trek.aotter.net/publisher/list/app) , create slot name and type.

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

![iOS_BannerAd](https://user-images.githubusercontent.com/46350143/120261545-ac031e80-c2ca-11eb-878d-9d18ad4e33ee.png)



## 2. create SuprAd object

For the publisher who use AotterTrek SDK for the first time and didn't get the full access of ad slot management, we provide the test key & ad place UUID. Checkout the value below.

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
// .m

#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>

@interface ViewController () <TKAdSuprAdDelegate>
{
    UIView *_suprAdView;
}
@property (nonatomic,strong) TKAdSuprAd *suprAd;
@end
.
.
.
@implementation ViewController
  
- (void)viewDidLoad {
    [super viewDidLoad];
    [self fetchSuprAd];
}

#pragma mark : fetch TKAdSuprAd

- (void)fetchSuprAd {
    
    if(!self.suprAd){
        // Replace the value in initWithPlace to your own UUID
        self.suprAd = [[TKAdSuprAd alloc] initWithPlace:@"YOUR UUID" category:@""];
    }
    self.suprAd.delegate = self;
  
    //register view controller for presenting
    [self.suprAd registerPresentingViewController:self];
    
    // fetch ad
    [self.suprAd fetchAd];
}
@end
```



## 3. received Ad datat with prefered ad size, register for adView and TKMediaView

```objective-c
-(void)TKAdSuprAd:(TKAdSuprAd *)suprAd didReceivedAdWithAdData:(NSDictionary *)adData preferedMediaViewSize:(CGSize)size isVideoAd{
  
    self.suprAd = suprAd;
    
	  //It is recommended to create the size of the view based on ad size.
    //prefered size for the ad，
    _suprAdView = [[UIView alloc]initWithFrame:CGRectMake(0, 0, size.width, size.height)];
    
    //AdView: the container view for ad
    [self.suprAd registerAdView:_suprAdView];
    [self.suprAd registerTKMediaView:_suprAdView];
    
    // AutoLayout
    [self.view addSubview:_suprAdView];
    
    [_suprAdView setTranslatesAutoresizingMaskIntoConstraints:NO];
    [_suprAdView.widthAnchor constraintEqualToConstant:size.width].active = YES;
    [_suprAdView.heightAnchor constraintEqualToConstant:size.height].active = YES;
    [_suprAdView.bottomAnchor constraintEqualToAnchor:self.view.safeAreaLayoutGuide.bottomAnchor].active = YES;
}
```



## 4. Ad load completed, the ad is ready to show on screen

```objective-c
- (void)TKAdSuprAdCompleted:(TKAdSuprAd *)suprAd{
    NSLog(@"TKAdSuprAd >> Completed");
}
```



## 5. Any error callback

```objective-c
- (void)TKAdSuprAd:(TKAdSuprAd *)suprAd adError:(TKAdError *)error{
    NSLog(@"TKAdSuprAd >> adError: %@", error.message);
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
[self.suprAd isExpired]; // YES or NO
```

### impression & click delegate

```objective-c
-(void)TKAdSuprAdWillLogImpression:(TKAdSuprAd *)ad{
    NSLog(@"TKSuprAd log impression");
}

-(void)TKAdSuprAdWillLogClick:(TKAdSuprAd *)ad{
    NSLog(@"TKSuprAd log click");
}
```

Github: [Banner Sample](https://github.com/aotter/AotterTrek-iOS-SDK/blob/master/AotterTrekSample/ViewController/DemoBannerAdViewController/DemoBannerAdViewController.m)

