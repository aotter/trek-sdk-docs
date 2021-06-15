# AdMob mediation intergration guide - Native

## Step 1. Create Ad slot

Enter the slot management of [Application List](https://trek.aotter.net/publisher/list/app) , create slot name and type.

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

Create Native Ad solt:

<img width="699" alt="nativead" src="https://user-images.githubusercontent.com/46350143/120260531-aa385b80-c2c8-11eb-82c1-43eea92c726f.png">

Create SuprAd solt:

<img width="695" alt="suprad" src="https://user-images.githubusercontent.com/46350143/120260535-aad0f200-c2c8-11eb-861d-b17cc9b1b1d6.png">



##  Step 2. set adMob mediation in your adMob mediation group panel 

If you have **Native Ad** needs, please follow the instructions implement

#### Class Name pleae enter: AotterTrekGADCustomEventNativeAd

#### Parameter:{"adType":"**nativeAd**", "adPlace":"<your ad place UUID>"}

The **adType** please fill in **nativeAd**，the **adPlace** please fill in your "**AotterTrek NativeAd adPlace**" .

We provide the test key & secret. Checkout the value below.

- CLIENT_ID : `21tgwWwuzFYiD4ko5Klr`
- CLIENT_SECRET : `fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44`
- Ad Place UUID: "YOUR ADPLACE UUID"

<img width="853" alt="Admob_native_noTestUUID" src="https://user-images.githubusercontent.com/46350143/122003417-a4249d80-cde5-11eb-8696-3055593b6c1a.png">



If you have **SuprAd** needs, please follow the instructions implement

#### Class Name pleae enter: AotterTrekGADCustomEventNativeAd

#### Parameter:{"adType":"**suprAd**", "adPlace":"<your ad place>"}

The **adType** please fill in **suprAd**，the **adPlace** please fill in your "**AotterTrek SuprAd adPlace**" .

We provide the test key & secret. Checkout the value below.

- CLIENT_ID : `21tgwWwuzFYiD4ko5Klr`
- CLIENT_SECRET : `fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44`
- Ad Place UUID: "YOUR ADPLACE UUID"

<img width="853" alt="Admon_suprad_noTestUUID" src="https://user-images.githubusercontent.com/46350143/122003425-a850bb00-cde5-11eb-9b93-089550ce2857.png">



##### Note:

For the publisher who use AotterTrek SDK for the first time and didn't get the full access of ad slot management, please contact Aseal representative or [E-mail to us](https://aseal.in/contactus).

Please **use your own** Client ID and Secret as well as UUID for production environment. Once you finishing this setting, you can switch **production / test mode** by changing **your client id and test client id.**

- CLIENT_ID : "YOUR CLIENT ID"

- CLIENT_SECRET : "YOUR CLIENT SECRET "

- Ad Place UUID: "YOUR ADPLACE UUID"

  

## Step 3. Install AdMob Mediation library in you project

File:Podfile 

The AdMob Mediation file sdk is "TrekSDKAdMobMediationObjc"，please follow the instructions to install sdk

**if you install GoogleMobileAds version 8 and above**

```objective-c
pod 'AotterTrek-iOS-SDK','3.6.0'
pod 'Google-Mobile-Ads-SDK','8.2.0'
pod 'TrekSDKAdMobMediationObjc','1.0.2' // Only support GoogleMobileAds version 8 and above
```

**if you install GoogleMobileAds version 7 and below**

```objective-c
pod 'AotterTrek-iOS-SDK','3.6.0'
pod 'Google-Mobile-Ads-SDK','7.69.0'
pod 'TrekSDKAdMobMediationObjc','1.0.1' // Only support GoogleMobileAds version 7 and below
```



## Step 4. render Google AdMob ads

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



### Customize TableViewCell (or CollectionViewCell)

Here we customize two TableViewCell: TrekNativeAdTableViewCell & TrekSuprAdTableViewCell

**Note:** View Class depends on the GoogleMobileAds SDK version

![UnifiedNativeAdView](https://user-images.githubusercontent.com/46350143/109942288-03671200-7d0f-11eb-8606-f5689a98ecec.png)

------

### Native Ad

File: TrekNativeAdTableViewCell.h

```objective-c
#import <UIKit/UIKit.h>
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>
#import <GoogleMobileAds/GoogleMobileAds.h>
#import <SDWebImage/SDWebImage.h>

NS_ASSUME_NONNULL_BEGIN

@interface TrekNativeAdTableViewCell : UITableViewCell

// Note: The declaration property depends on the SDK version
@property(nonatomic, strong) GADUnifiedNativeAdView *nativeAdView; 	// GoogleMobileAds version 8 below
@property(nonatomic, strong) GADNativeAdView *nativeAdView;					// GoogleMobileAds version 8 above

- (void)setGADUnifiedNativeAdData:(GADUnifiedNativeAd *)nativeAd;

@end

NS_ASSUME_NONNULL_END
```



### Declare a GADUnifiedNativeAd data method:

File: TrekNativeAdTableViewCell.m

```objective-c

// The implementation method depends on the SDK version

// Google Mediation NativeAd

// GoogleMobileAds version 8 below
- (void)setGADUnifiedNativeAdData:(GADUnifiedNativeAd *)nativeAd {

    NSArray *nibObjects =
    [[NSBundle mainBundle] loadNibNamed:@"UnifiedNativeAdView" owner:nil options:nil];
    [self setAdView:[nibObjects firstObject]];

    ((UIImageView *)self.nativeAdView.iconView).image = nativeAd.icon.image;
    ((UILabel *)self.nativeAdView.headlineView).text = nativeAd.headline;
    ((UILabel *)self.nativeAdView.bodyView).text = nativeAd.body;
    ((UILabel *)self.nativeAdView.advertiserView).text = nativeAd.advertiser;
    self.nativeAdView.nativeAd = nativeAd;

    [self addSubview:self.nativeAdView];
}

// GoogleMobileAds version 8 above
- (void)setGADNativeAdData:(GADNativeAd *)nativeAd {
    
    NSArray *nibObjects =
    [[NSBundle mainBundle] loadNibNamed:@"UnifiedNativeAdView" owner:nil options:nil];
    [self setAdView:[nibObjects firstObject]];

    ((UIImageView *)self.nativeAdView.iconView).image = nativeAd.icon.image;
    ((UILabel *)self.nativeAdView.headlineView).text = nativeAd.headline;
    ((UILabel *)self.nativeAdView.bodyView).text = nativeAd.body;
    ((UILabel *)self.nativeAdView.advertiserView).text = nativeAd.advertiser;
    self.nativeAdView.nativeAd = nativeAd;

    [self addSubview:self.nativeAdView];
}


// Common Method
- (void)setAdView:(GADUnifiedNativeAdView *)view {
    // Remove previous ad view.
    [self.nativeAdView removeFromSuperview];
    self.nativeAdView = view;

    // Add new ad view and set constraints to fill its container.
    [self addSubview:view];
    [self.nativeAdView setTranslatesAutoresizingMaskIntoConstraints:NO];

    NSDictionary *viewDictionary = NSDictionaryOfVariableBindings(_nativeAdView);
    [self addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|[_nativeAdView]|"
                                                                      options:0
                                                                      metrics:nil
                                                                        views:viewDictionary]];
    [self addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|[_nativeAdView]|"
                                                                      options:0
                                                                      metrics:nil
                                                                        views:viewDictionary]];
}

```



# File: TrekNativeAdTableViewCell.xib

![TrekNativeAdTableViewCell](https://user-images.githubusercontent.com/46350143/109943071-c2233200-7d0f-11eb-894f-2ccd4ff701ee.png)

------

### Supr Ad

File: TrekSuprAdTableViewCell.h

```objective-c
#import <UIKit/UIKit.h>
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>
#import <GoogleMobileAds/GoogleMobileAds.h>

NS_ASSUME_NONNULL_BEGIN

@interface TrekSuprAdTableViewCell : UITableViewCell

// Note: The declaration method depends on the SDK version
// GoogleMobileAds version 8 below
- (void)setGADUnifiedNativeAdData:(GADUnifiedNativeAd *)nativeAd withViewSize:(CGSize)size;

// GoogleMobileAds version 8 above
- (void)setGADNativeAdData:(GADNativeAd *)nativeAd withViewSize:(CGSize)size;

@end

NS_ASSUME_NONNULL_END
```




### Declare a GADUnifiedNativeAd data method:

File: TrekSuprAdTableViewCell.m

```objective-c
// The implementation method depends on the SDK version

// Google Mediation SuprAD

// GoogleMobileAds version 8 below
- (void)setGADUnifiedNativeAdData:(GADUnifiedNativeAd *)nativeAd withViewSize:(CGSize)size {
    
    // Clear contentView subviews
    for (UIView *subView in self.contentView.subviews) {
        [subView removeFromSuperview];
    }
    
    //self.contentView.backgroundColor = [UIColor redColor];
    
    CGRect rect = CGRectMake(0, 0, size.width, size.height);
    GADMediaView *gADMediaView = [[GADMediaView alloc]initWithFrame:rect];
    gADMediaView.mediaContent = nativeAd.mediaContent;
    [self.contentView addSubview:gADMediaView];

    [gADMediaView setTranslatesAutoresizingMaskIntoConstraints:NO];

    [gADMediaView setTranslatesAutoresizingMaskIntoConstraints:NO];
    [gADMediaView.leadingAnchor constraintEqualToAnchor:self.contentView.leadingAnchor].active = YES;
    [gADMediaView.trailingAnchor constraintEqualToAnchor:self.contentView.trailingAnchor].active = YES;
    [gADMediaView.topAnchor constraintEqualToAnchor:self.contentView.topAnchor].active = YES;
    [gADMediaView.bottomAnchor constraintEqualToAnchor:self.contentView.bottomAnchor].active = YES;
}


// GoogleMobileAds version 8 above
- (void)setGADNativeAdData:(GADNativeAd *)nativeAd withViewSize:(CGSize)size {
    
    for (UIView *subView in self.contentView.subviews) {
        [subView removeFromSuperview];
    }

    //self.contentView.backgroundColor = [UIColor redColor];

    CGRect rect = CGRectMake(0, 0, size.width, size.height);
    GADMediaView *gADMediaView = [[GADMediaView alloc]initWithFrame:rect];
    gADMediaView.mediaContent = nativeAd.mediaContent;
    [self.contentView addSubview:gADMediaView];

    [gADMediaView setTranslatesAutoresizingMaskIntoConstraints:NO];

    [gADMediaView setTranslatesAutoresizingMaskIntoConstraints:NO];
    [gADMediaView.leadingAnchor constraintEqualToAnchor:self.contentView.leadingAnchor].active = YES;
    [gADMediaView.trailingAnchor constraintEqualToAnchor:self.contentView.trailingAnchor].active = YES;
    [gADMediaView.topAnchor constraintEqualToAnchor:self.contentView.topAnchor].active = YES;
    [gADMediaView.bottomAnchor constraintEqualToAnchor:self.contentView.bottomAnchor].active = YES;
}
```



File:TrekSuprAdTableViewCell.xib

![TrekSuprAdTableViewCel](https://github.com/aotter/trek-sdk-docs/wiki/GoogleAdMobMediationSuprAD/TrekSuprAdTableViewCell.png)



File: YourViewController.h

YourViewController that rendering the AdView(include NativeAd & SuprAd) process:

```objective-c
// Define the display position of the ad in the TableView
static NSInteger googleMediationNativeAdPosition = 2;
static NSInteger googleMediationSuprAdPosition = 6;
.
.
.

// GoogleMobileAds version 8 above, the delegate is "GADNativeAdLoaderDelegate"
// GoogleMobileAds version 8 below, the delegate is "GADUnifiedNativeAdLoaderDelegate"
  
@interface YourViewController ()<GADUnifiedNativeAdLoaderDelegate, UITableViewDataSource, UITableViewDelegate> {
  
		// For NativeAd
    GADUnifiedNativeAd *_gADUnifiedNativeAd;  // GoogleMobileAds version 8 below
    GADNativeAd *_gADUnifiedNativeAd;					// GoogleMobileAds version 8 above

    // For SuprAd
    GADUnifiedNativeAd *_gADUnifiedSuprAd;	  // GoogleMobileAds version 8 below
    GADNativeAd *_gADUnifiedSuprAd;						// GoogleMobileAds version 8 above
}

@implementation YourViewController

- (void)viewDidLoad {
    [super viewDidLoad];
  
    [self setupTableVie];
    [self setupGADAdLoader];
}

#pragma mark : Setup TableView

- (void)setupTableVie {
    self.adTableView.dataSource = self;
    self.adTableView.delegate = self;
    
    
    [self.adTableView registerClass:UITableViewCell.class forCellReuseIdentifier:@"Cell"];
    
    [self.adTableView registerNib:[UINib nibWithNibName:@"TrekNativeAdTableViewCell" bundle:nil] forCellReuseIdentifier:@"TrekNativeAdTableViewCell"];
    
    [self.adTableView registerNib:[UINib nibWithNibName:@"TrekSuprAdTableViewCell" bundle:nil] forCellReuseIdentifier:@"TrekSuprAdTableViewCell"];
}

#pragma mark : Setup GADAdLoader

- (void)setupGADAdLoader {
    
    // GoogleMobileAds version 8 below
    self.adLoader = [[GADAdLoader alloc]initWithAdUnitID: @"<your adUnit Id>"
                                      rootViewController: self
                                                 adTypes: @[kGADAdLoaderAdTypeUnifiedNative]
                                                 options: @[]];
  
    // GoogleMobileAds version 8 above
    self.adLoader = [[GADAdLoader alloc]initWithAdUnitID: @"<your adUnit Id>"
                                      rootViewController: self
                                                 adTypes: @[kGADAdLoaderAdTypeNative]
                                                 options: @[]];
    
    self.adLoader.delegate = self;

    [self adLoaderLoadRequest];
}

- (void)adLoaderLoadRequest {
    [self.adLoader loadRequest:[GADRequest request]];
}

#pragma mark - UITableViewDataSource

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return 30;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
     
    // For NativeAd
    if (indexPath.row == googleMediationNativeAdPosition) {
        if (_gADUnifiedNativeAd != nil) {
            TrekNativeAdTableViewCell *trekNativeAdTableViewCell = [tableView dequeueReusableCellWithIdentifier:@"TrekNativeAdTableViewCell" forIndexPath:indexPath];
          
            // GoogleMobileAds version 8 below
            [trekNativeAdTableViewCell setGADUnifiedNativeAdData:_gADUnifiedNativeAd];
          
            // GoogleMobileAds version 8 above
            [trekNativeAdTableViewCell setGADNativeAdData:_gADUnifiedNativeAd];
          
            return trekNativeAdTableViewCell;
        }
    }
    
    // For SuprAd
    if (indexPath.row == googleMediationSuprAdPosition) {
      
      if(_gADUnifiedSuprAd != nil) {
            TrekSuprAdTableViewCell *trekSuprAdTableViewCell = [tableView dequeueReusableCellWithIdentifier:@"TrekSuprAdTableViewCell" forIndexPath:indexPath];
            
            if ([[_gADUnifiedSuprAd.extraAssets allKeys]containsObject:@"adSizeWidth"] &&
                [[_gADUnifiedSuprAd.extraAssets allKeys]containsObject:@"adSizeHeight"]) {
                
                // get ad prefered AdSize
                NSString *width = _gADUnifiedSuprAd.extraAssets[@"adSizeWidth"];
                NSString *height = _gADUnifiedSuprAd.extraAssets[@"adSizeHeight"];
                double adSizeWidth = [width doubleValue];
                double adSizeHeight = [height doubleValue];

                CGFloat viewWidth = UIScreen.mainScreen.bounds.size.width;
                CGFloat viewHeight = viewWidth * adSizeHeight/adSizeWidth;
                int adheight = (int)viewHeight;
                CGSize preferedMediaViewSize = CGSizeMake(viewWidth, adheight);
                
                // GoogleMobileAds version 8 below
            		[trekSuprAdTableViewCell setGADUnifiedNativeAdData:_gADUnifiedSuprAd withViewSize:size];
          
          			// GoogleMobileAds version 8 above
            		[trekSuprAdTableViewCell setGADNativeAdData:_gADUnifiedSuprAd withViewSize:size];
            }
            
            return trekSuprAdTableViewCell;
        }
    }
    
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"Cell" forIndexPath:indexPath];
    cell.textLabel.text = [[NSString alloc]initWithFormat:@"index:%ld",(long)indexPath.row];
    return  cell;
}

#pragma mark - UITableViewDelegate

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {

		// For NativeAd
    if (indexPath.row == googleMediationNativeAdPosition) {
        return _gADUnifiedNativeAd == nil ? 0:80;
    }
    
    // For SuprAd
    if (indexPath.row == googleMediationSuprAdPosition) {
        
        if ([[_gADUnifiedSuprAd.extraAssets allKeys]containsObject:@"adSizeWidth"] &&
            [[_gADUnifiedSuprAd.extraAssets allKeys]containsObject:@"adSizeHeight"]) {
            
            // get ad prefered AdSize
            NSString *width = _gADUnifiedSuprAd.extraAssets[@"adSizeWidth"];
            NSString *height = _gADUnifiedSuprAd.extraAssets[@"adSizeHeight"];
            double adSizeWidth = [width doubleValue];
            double adSizeHeight = [height doubleValue];

            CGFloat viewWidth = UIScreen.mainScreen.bounds.size.width;
            CGFloat viewHeight = viewWidth * adSizeHeight/adSizeWidth;
            
            return _gADUnifiedSuprAd == nil ? 0:viewHeight;
        }
    }
  
    return 80;
}



// GoogleMobileAds version 8 below

#pragma mark - GADUnifiedNativeAdLoaderDelegate

- (void)adLoader:(nonnull GADAdLoader *)adLoader didReceiveUnifiedNativeAd:(nonnull GADUnifiedNativeAd *)nativeAd {
    
    // Delegate 回來的 nativeAd 已經可以接取到自己的 Custom Ad View，
    // 在這裡我將 nativeAd 放到 CustomTableViewCell 去接資料
    
    if (nativeAd != nil) {
        
        if ([[nativeAd.extraAssets allKeys]containsObject:@"trekAd"]) {
            NSString *key = nativeAd.extraAssets[@"trekAd"];
            
            if ([key isEqualToString:@"nativeAd"]) {
                _gADUnifiedNativeAd = nativeAd;
            }else if ([key isEqualToString:@"suprAd"]) {
                _gADUnifiedSuprAd = nativeAd;
            }
        }
    }
    
    [self.adTableView reloadData];
}

- (void)adLoader:(nonnull GADAdLoader *)adLoader didFailToReceiveAdWithError:(nonnull GADRequestError *)error {
    NSLog(@"Error Message:%@",error.description);
}



// GoogleMobileAds version 8 above

#pragma mark - GADNativeAdLoaderDelegate

- (void)adLoader:(GADAdLoader *)adLoader didReceiveNativeAd:(GADNativeAd *)nativeAd {
    
    // Delegate 回來的 nativeAd 已經可以接取到自己的 Custom Ad View，
    // 這部分可以將 nativeAd 放到 CustomTableViewCell 去接資料

    if (nativeAd != nil) {

        if ([[nativeAd.extraAssets allKeys]containsObject:@"trekAd"]) {
            NSString *adType = nativeAd.extraAssets[@"trekAd"];

            if ([adType isEqualToString:@"nativeAd"]) {
                _gADUnifiedNativeAd = nativeAd;
            }else if ([key isEqualToString:@"suprAd"]) {
                _gADUnifiedSuprAd = nativeAd;
            }
        }
    }

    [self.adTableView reloadData];
}

- (void)adLoader:(GADAdLoader *)adLoader didFailToReceiveAdWithError:(NSError *)error {
    NSLog(@"Error Message:%@",error.description);
}


@end
```

# Step 5. Additional Method for SuprAd (AdScrolled)

The AotterTrek's SuprAd type ad need to be notified when the ad view is scrolled, in order to show some specfic view according to the position of the ad.
Therefore, you should add additional method in following:

File: ViewController.m (Can use your Custom ViewController)

```objective-c
#pragma mark : ScrlloView delegate

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    if (_gADUnifiedSuprAd != nil) {
      
        // The postNotificationName please fill in "SuprAdScrolled"
        [[NSNotificationCenter defaultCenter]postNotificationName:@"SuprAdScrolled"
                                                           object:self
                                                         userInfo:nil];
    }
}
```



## Note :  

- **If you project based on Swift，and you have SuprAd needs**

**Step1**. You need to write "#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>" in the bridge file

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
