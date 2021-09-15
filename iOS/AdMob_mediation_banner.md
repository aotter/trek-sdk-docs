# AdMob mediation intergration guide - Banner

## Step 1. Create Ad slot

Enter the slot management of [Application List](https://trek.aotter.net/publisher/list/app) , create slot name and type.

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

![iOS_BannerAd](https://user-images.githubusercontent.com/46350143/120261545-ac031e80-c2ca-11eb-878d-9d18ad4e33ee.png)



##  Step 2. set adMob mediation in your adMob mediation group panel 

#### Class Name pleae enter: AotterTrekGADCustomEventBannerAd

#### Parameter:{"adType":"**suprAd**", "adPlace":"<your ad place UUID>"}

The **adType** please fill in **suprAd**，the **adPlace** please fill in your "**AotterTrek Banner adPlace**" .

For the publisher who use AotterTrek SDK for the first time and didn't get the full access of ad slot management, please contact Aseal representative or [E-mail to us](https://aseal.in/contactus).

We provide the test key & secret. Checkout the value below.

- CLIENT_ID : `21tgwWwuzFYiD4ko5Klr`
- CLIENT_SECRET : `fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44`
- Ad Place UUID: "YOUR ADPLACE UUID"

<img width="862" alt="Admob_banner_noTestUUID" src="https://user-images.githubusercontent.com/46350143/122003427-a8e95180-cde5-11eb-8f93-da789d4e9525.png">

##### Note:

Please **use your own** Client ID and Secret as well as UUID for production environment. Once you finishing this setting, you can switch **production / test mode** by changing **your client id and test client id.**

- CLIENT_ID : "YOUR CLIENT ID"
- CLIENT_SECRET : "YOUR CLIENT SECRET "
- Ad Place UUID: "YOUR ADPLACE UUID"

## Step 3. Install AdMob Mediation library in you project

File:Podfile 

The AdMob Mediation file sdk is "TrekSDKAdMobMediationObjc"，please follow the instructions to install sdk

**if you install GoogleMobileAds version 8 and above**

```objective-c
pod 'AotterTrek-iOS-SDK','3.6.2'
pod 'Google-Mobile-Ads-SDK','8.8.0'
pod 'TrekSDKAdMobMediationObjc','1.0.4' // Only support GoogleMobileAds version 8 and above
```

**if you install GoogleMobileAds version 7 and below**

```objective-c
pod 'AotterTrek-iOS-SDK','3.6.2'
pod 'Google-Mobile-Ads-SDK','7.69.0'
pod 'TrekSDKAdMobMediationObjc','1.0.3' // Only support GoogleMobileAds version 7 and below
```

TrekSDKAdMobMediationObjc [version](https://github.com/aotter/aotter-trek-admob-mediation-public-ios-objc/blob/master_admob_mediation_v8/README.md)

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
    
    [self bannerLoadRequest];
}

-(void)bannerLoadRequest {
    GADRequest *request = [GADRequest request];
    GADCustomEventExtras *extra = [[GADCustomEventExtras alloc] init];
  
  	// The key please fill in "category"
  	// YOU_CATEGORY like "news"、"movie"
    [extra setExtras:@{@"category":@"YOUR_CATEGORY"} forLabel:@"AotterTrekGADCustomEventBannerAd"];
    [request registerAdNetworkExtras:extra];
    
    [_gadBannerView loadRequest:request];
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