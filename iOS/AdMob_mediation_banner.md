# AdMob mediation intergration guide - Banner

## Step 1. Create Ad slot

Enter the slot management of [Application List](https://trek.aotter.net/publisher/list/app) , create slot name and type.

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

![iOS_BannerAd](https://user-images.githubusercontent.com/46350143/120261545-ac031e80-c2ca-11eb-878d-9d18ad4e33ee.png)



##  Step 2. set adMob mediation in your adMob mediation group panel 

#### Class Name pleae enter: AotterTrekGADCustomEventBannerAd

#### Parameter:{"adType":"**suprAd**", "adPlace":"<your ad place>"}

The **adType** please fill in **suprAd**，the **adPlace** please fill in your "**AotterTrek Banner adPlace**" .

If you want to test, please use the test key & adPlace below，

- CLIENT_ID : `21tgwWwuzFYiD4ko5Klr`
- CLIENT_SECRET : `fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44`
- adPlace: `banner`

<img width="862" alt="AdmMob CustomEvent" src="https://user-images.githubusercontent.com/46350143/120164333-0baaec80-c22d-11eb-9192-220c7d03e513.png">



## Step 3. Install AdMob Mediation library in you project

File:Podfile 

The AdMob Mediation file sdk is "TrekSDKAdMobMediationObjc"，please follow the instructions to install sdk

**if you install GoogleMobileAds version 8 and above**

```objective-c
pod 'AotterTrek-iOS-SDK','3.5.9'
pod 'Google-Mobile-Ads-SDK','8.2.0'
pod 'TrekSDKAdMobMediationObjc','1.0.2' // Only support GoogleMobileAds version 8 and above
```

**if you install GoogleMobileAds version 7 and below**

```objective-c
pod 'AotterTrek-iOS-SDK','3.5.9'
pod 'Google-Mobile-Ads-SDK','7.69.0'
pod 'TrekSDKAdMobMediationObjc','1.0.1' // Only support GoogleMobileAds version 7 and below
```



## Step 4. render Google AdMob Banner ads

### Initial The AotterTrek SDK

File: AppDelegate.m

```objective-c
/// Need to import Lib
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>
#import <GoogleMobileAds/GoogleMobileAds.h>
.
.
.

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
   [[GADMobileAds sharedInstance] startWithCompletionHandler:nil];
    
  [[AotterTrek sharedAPI] initTrekServiceWithClientId:@"Your Client ID"
                                                   secret:@"Your secret"];
    // Open Log
    //[[AotterTrek sharedAPI] performSelector:@selector(enableLoggerLevelDevDetail)];
    
    return YES;
}

```

File: YourViewController.m

```objective-c
//Ex:BannerViewController
#import "BannerViewController.h"
#import <GoogleMobileAds/GoogleMobileAds.h>

static NSString *const BannerAdUnit = @"Your AdMob banner ad unit";

@interface BannerViewController ()<GADBannerViewDelegate> {
    GADBannerView *_gadBannerView;
}
@end

@implementation BannerViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view from its nib.
    
    [self setupGADBannerView];
}

#pragma mark : Setup GADBanner

- (void)setupGADBannerView {
    _gadBannerView = [[GADBannerView alloc]initWithAdSize:kGADAdSizeBanner];
    _gadBannerView.delegate = self;
    _gadBannerView.rootViewController = self;
    _gadBannerView.adUnitID = BannerAdUnit;
    [_gadBannerView loadRequest:[GADRequest request]];
}

- (void)setupBannerViewUI:(GADBannerView *)bannerView {
    
    [self.view addSubview:bannerView];

    [bannerView setTranslatesAutoresizingMaskIntoConstraints:NO];

    [bannerView.heightAnchor constraintEqualToConstant:bannerView.bounds.size.height].active = YES;
    [bannerView.bottomAnchor constraintEqualToAnchor:self.view.safeAreaLayoutGuide.bottomAnchor constant:0.0].active = YES;
    [bannerView.leftAnchor constraintEqualToAnchor:self.view.leftAnchor constant:0].active = YES;
    [bannerView.rightAnchor constraintEqualToAnchor:self.view.rightAnchor constant:0].active = YES;
}


// if you install GoogleMobileAds version 8 and above
#pragma mark :GADBannerViewDelegate
- (void)bannerViewDidReceiveAd:(GADBannerView *)bannerView {
    if (bannerView != nil) {
        [self setupBannerViewUI:bannerView];
    }
}

- (void)bannerView:(GADBannerView *)bannerView didFailToReceiveAdWithError:(NSError *)error {
    NSLog(@"error message: %@",error.localizedDescription);
}


// if you install GoogleMobileAds version 7 and below 
- (void)adViewDidReceiveAd:(GADBannerView *)bannerView {
    if (bannerView != nil) {
        [self setupBannerViewUI:bannerView];
    }
}

- (void)adView:(GADBannerView *)bannerView didFailToReceiveAdWithError:(GADRequestError *)error {
    NSLog(@"error message: %@",error.localizedDescription);
}

@end
```



## Note :  

- **If you project based on Swift**

**Step1**. You need to write "**#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>**" in the **bridge file**

```objective-c
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>
```

- Supr.Ad including video ad advertising, which is using VAST technology provided by the Google IMA SDK. In the implementation of VAST, Trek SDK will need to register ViewController when request VAST ads from Google IMA. This ViewController should be the same ViewController that displays the video ads!

  EX:

  A viewController requests ads, A viewController displays ads (o)

  A viewController requests ads, B viewController displays ads (x)

---

## Demo App:

If you want to download Demo app, you need to configure **GADApplicationIdentifier** and **Ad Unit** in projects.

GoogleMobileAds version 8 and above: [Sample project for v8](https://github.com/aotter/trek-ios-AdMobMediation_v8-sample)

GoogleMobileAds version 7 and below: [Sample project for v7](https://github.com/aotter/trek-ios-AdMobMediation_v7-sample)

If you use sample project，please read the **README.md**

[Sample project for v8 README](https://github.com/aotter/trek-ios-AdMobMediation_v8-sample/blob/main/README.md)

[Sample project for v7 README](https://github.com/aotter/trek-ios-AdMobMediation_v7-sample/blob/main/README.md)



File: YourViewController.m

![Ad Unit](https://github.com/aotter/trek-sdk-docs/wiki/GoogleAdMobMediationSuprAD/GoogleMediationDemoAppViewController.png)

File: Info.plist

![GADApplicationIdentifier](https://github.com/aotter/trek-sdk-docs/wiki/GoogleAdMobMediationSuprAD/GoogleMediationDemoAppInfo.png)

File: AppDelegate.m

Please fill in your Client ID and Secret.

<img width="726" alt="ClienID_Objc" src="https://user-images.githubusercontent.com/46350143/120173914-1ff3e700-c237-11eb-9d55-9af9f6bcb243.png">