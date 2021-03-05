# AdMob mediation intergration guide

## Step 1. prepare Custon Event file for AotterTrek

follow the official Guide [here](https://developers.google.com/admob/ios/native/native-custom-events) (recommend)
or just download the file and add files to your projects from folder.

If your project based on Objective-C，

Download: [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Aotter_GoogleMediation_version7_project_Objc.zip)  with googleAds SDK version 8 below.

Download: [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Aotter_GoogleMediation_version8_project_Objc.zip)  with googleAds SDK version 8 above.

If your project based on Swift，

Download: [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Aotter_GoogleMediation_version7_project_Swift.zip)  with googleAds SDK version 8 below.

Download: [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Aotter_GoogleMediation_version8_project_Swift.zip)  with googleAds SDK version 8 above.



##  Step 2. set adMob mediation in your adMob mediation group panel 

Class Name: AotterTrekGADCustomEventNativeAd (or the file title you created)
Parameter: {"adType":"nativeAd", "adPlace":"<your ad place>"}
![AdMob Setting](https://user-images.githubusercontent.com/3615917/102583345-7fee4980-413f-11eb-9538-4304a18432db.png)



## Step 3. render Google AdMob ads

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
    
    [[AotterTrek sharedAPI] initTrekServiceWithClientId:@"21tgwWwuzFYiD4ko5Klr"
                                                   secret:@"fD8P20gzWYrlbuwWklRkicYcNwlWZSZwV+iHj3TzGSzzyfgTWmVR5trs5F1Dp+x9tX2jxq44"];
    
    // Open Log
    //[[AotterTrek sharedAPI] performSelector:@selector(enableLoggerLevelDevDetail)];
    
    return YES;
}

```



### Customize TableViewCell (or CollectionViewCell)

Here we customize two TableViewCell: TrekNativeAdTableViewCell & TrekSuprAdTableViewCell

**Note:** View Class depends on the GoogleMobileAds SDK version

![UnifiedNativeAdView](https://user-images.githubusercontent.com/46350143/109942288-03671200-7d0f-11eb-8606-f5689a98ecec.png)



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
  
    CGFloat _viewWidth;
    CGFloat _viewHeight;
}

@implementation YourViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    _viewWidth = UIScreen.mainScreen.bounds.size.width;
    _viewHeight = _viewWidth * 9/16;
    
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
            
            CGSize size = CGSizeMake(_viewWidth, _viewHeight);
            
          	// GoogleMobileAds version 8 below
            [trekSuprAdTableViewCell setGADUnifiedNativeAdData:_gADUnifiedSuprAd withViewSize:size];
          
          	// GoogleMobileAds version 8 above
            [trekSuprAdTableViewCell setGADNativeAdData:_gADUnifiedSuprAd withViewSize:size];
          
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
        return _gADUnifiedSuprAd == nil ? 0:_viewHeight;
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

- (void)adLoader:(GADAdLoader *)adLoader didFailToReceiveAdWithError:(NSError *)error {
    NSLog(@"Error Message:%@",error.description);
}


@end
```



# Step 4. Additional Method for SuprAd (AdScrolled)

The AotterTrek's SuprAd type ad need to be notified when the ad view is scrolled, in order to show some specfic view according to to the position of the ad.
Therefore, you should add additional method in following:

File: YourViewController.h (the ViewController that rendering the AdView)

```objective-c
#import <UIKit/UIKit.h>

@protocol YourViewControllerDelegate <NSObject>

- (void)rootViewControllerScrollViewDidScroll:(UIScrollView *)scrollView;

@end

@interface YourViewController : UIViewController

@property (weak, nonatomic) IBOutlet UITableView *adTableView;

@property id<YourViewControllerDelegate> delegate;

@end
```



File: ViewController.m (Can use your Custom ViewController)

```objective-c
#pragma mark : ScrlloView delegate

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
		if (_gADUnifiedSuprAd != nil) {
        [self.delegate rootViewControllerScrollViewDidScroll:scrollView];
    }
}
```



### CustomViewController need to modify your ViewController class.

File: AotterTrekGADCustomEventNativeAd.m

```objective-c
@interface AotterTrekGADCustomEventNativeAd()<ViewControllerDelegate> {

		// 部分參數忽略
		.
		.
		.
		
    // CustomViewController 隨著自己定義的 ViewController 來決定
    // EX: SomeViewController (Your Custom ViewController)
    // declared: SomeViewController *_customViewController;
    YourViewController *_customViewController;
}


@implementation AotterTrekGADCustomEventNativeAd

- (void)requestNativeAdWithParameter:(NSString *)serverParameter request:(GADCustomEventRequest *)request adTypes:(NSArray *)adTypes options:(NSArray *)options rootViewController:(UIViewController *)rootViewController {
    
    // Handle rootViewController delegate, need to transfer your custom ViewController
    // EX:
    // _customViewController = (SomeViewController *) rootViewController;
    // This delegate is your rootViewController delegate
    
    _customViewController = (ViewController *) rootViewController;
    _customViewController.delegate = self;
    
    // 部分程式忽略
    .
    .
    .
}

#pragma mark - ViewControllerDelegate (CustomViewController Scroll Delegate)

- (void)rootViewControllerScrollViewDidScroll:(UIScrollView *)scrollView {
    if (_suprAd != nil) {
        [_suprAd notifyAdScrolled];
    }
}

@end
```



## Note :  

**If you project based on Swift，and you have SuprAd needs**

**Step1**. You need to write "#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>" in the bridge file

```
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>
```



**Step2.** Your CustomViewController will be modified as shown in the figure

<img width="914" alt="CustomViewController" src="https://user-images.githubusercontent.com/46350143/109950474-812f1b80-7d17-11eb-802f-70fe3c4dfcc7.png">



**Step3.** Find the **"AotterTrekGADCustomEventNativeAd.m"** file，please import the bridge file in order to call the CustomViewController delegate

```
#import "Your Bridge file Name-Swift.h"
```

See the below，

<img width="852" alt="bridge" src="https://user-images.githubusercontent.com/46350143/110062479-47f0bd00-7da4-11eb-9e3b-b0c86d30c27c.png">

---

## Demo App:

If you want to download Demo app, you need to configure GADApplicationIdentifier and Ad Unit in projects.

If your project based on Objective-C，

GoogleMobileAds version 8 below: [Download](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Objc_GoogleMobileAds_version7.zip)

GoogleMobileAds version 8 above: [Download](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Objc_GoogleMobileAds_version8.zip)



If your project based on Swift，

GoogleMobileAds version 8 below: [Download](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Swift_GoogleMobileAds_version7.zip)

GoogleMobileAds version 8 above: [Download](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.7/Swift_GoogleMobileAds_version8.zip)



File: ViewController.m

![Ad Unit](https://github.com/aotter/trek-sdk-docs/wiki/GoogleAdMobMediationSuprAD/GoogleMediationDemoAppViewController.png)

File: Info.plist

![GADApplicationIdentifier](https://github.com/aotter/trek-sdk-docs/wiki/GoogleAdMobMediationSuprAD/GoogleMediationDemoAppInfo.png)

