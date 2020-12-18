# AdMob mediation intergration guide

! only support TKAdNative now !

## Step 1. prepare Custon Event file for AotterTrek

follow the Ofiicial Guiide [here](https://developers.google.com/admob/ios/native/native-custom-events) (recommend)

or just download the file we prepared [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.3.5/AotterTrek-iOS-SDK.framework.zip) and add files to your projects from "AotterTrek adMob mediation file" folder



##  Step 2. set adMob mediation in your adMob mediation group panel 

Class Name: AotterTrekGADCustomEventNativeAd (or the file title you created)

Parameter: {"adType":"nativeAd", "adPlace":"<your ad place>"}



## Step 3. render Google AdMob ads

follow the [Offical Guilde](AotterTrek adMob mediation file) to render google ads through mediation.

Download:  [AotterTrek.adMob.mediation.zip](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.5.3/AotterTrek.adMob.mediation.zip)

Add AotterTrekGADMediation files to your project.

File: AppDelegate.m

```
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



Custom TableViewCell (or CollectionViewCell)

Here we custom two TableViewCell: TrekNativeAdTableViewCell & TrekSuprAdTableViewCell

File: TrekNativeAdTableViewCell.h

```
#import <UIKit/UIKit.h>
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>
#import <GoogleMobileAds/GoogleMobileAds.h>
#import <SDWebImage/SDWebImage.h>

NS_ASSUME_NONNULL_BEGIN

@interface TrekNativeAdTableViewCell : UITableViewCell

@property (weak, nonatomic) IBOutlet UIImageView *adImageView;
@property (weak, nonatomic) IBOutlet UILabel *adTilteLabel;
@property (weak, nonatomic) IBOutlet UILabel *adDescrLabel;
@property (weak, nonatomic) IBOutlet UILabel *sponsoredLabel;

- (void)setGADUnifiedNativeAdData:(GADUnifiedNativeAd *)nativeAd;

@end

NS_ASSUME_NONNULL_END
```



File: TrekNativeAdTableViewCell.m

Declared a GADUnifiedNativeAd data method:

```
// Google Mediation NativeAd

- (void)setGADUnifiedNativeAdData:(GADUnifiedNativeAd *)nativeAd {
    
    // Use "SDWebImage" Third-party
    [self.adImageView sd_setImageWithURL:nativeAd.icon.imageURL];
    
    self.adTilteLabel.text = nativeAd.headline;
    self.adDescrLabel.text = nativeAd.body;
    self.sponsoredLabel.text = [NSString stringWithFormat:@"%@ | %@",
                                @"贊助",
                                nativeAd.advertiser];
}
```



File: TrekNativeAdTableViewCell.xib

![TrekNativeAdTableViewCell](./GoogleAdMobMediationSuprAD/TrekNativeAdTableViewCell.png)

File: TrekSuprAdTableViewCell.h

```
#import <UIKit/UIKit.h>
#import <AotterTrek-iOS-SDK/AotterTrek-iOS-SDK.h>
#import <GoogleMobileAds/GoogleMobileAds.h>

NS_ASSUME_NONNULL_BEGIN

@interface TrekSuprAdTableViewCell : UITableViewCell

- (void)setGADUnifiedNativeAdData:(GADUnifiedNativeAd *)nativeAd withViewSize:(CGSize)size;

@end

NS_ASSUME_NONNULL_END
```



File: TrekSuprAdTableViewCell.m

Declared a GADUnifiedNativeAd data method:

```
// Google Mediation SuprAD

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
```



File:TrekSuprAdTableViewCell.xib

![TrekSuprAdTableViewCel](./GoogleAdMobMediationSuprAD/TrekSuprAdTableViewCell.png)



File: ViewController.h (Can use your Custom ViewController)

If you ready to use SuprAd advertising type, need to implement delegate:

```
#import <UIKit/UIKit.h>

@protocol ViewControllerDelegate <NSObject>

- (void)rootViewControllerScrollViewDidScroll:(UIScrollView *)scrollView;

@end

@interface ViewController : UIViewController

@property (weak, nonatomic) IBOutlet UITableView *adTableView;

@property id<ViewControllerDelegate> delegate;

@end
```



File: ViewController.m (Can use your Custom ViewController)

If you ready to use SuprAd advertising type, need to implement ScrlloView delegate, the reason is that AotterTrek SuprAd needs to generate part of SuprAd's interactive behavior when your App has a sliding behavior. 

```
// Define the display position of the ad in the TableView
static NSInteger googleMediationNativeAdPosition = 2;
static NSInteger googleMediationSuprAdPosition = 6;
.
.
.

@interface ViewController ()<GADUnifiedNativeAdLoaderDelegate, UITableViewDataSource, UITableViewDelegate> {
		// For NativeAd
    GADUnifiedNativeAd *_gADUnifiedNativeAd; 
    
    // For SuprAd
    GADUnifiedNativeAd *_gADUnifiedSuprAd;
    CGFloat _viewWidth;
    CGFloat _viewHeight;
}

@implementation ViewController

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
    
    self.adLoader = [[GADAdLoader alloc]initWithAdUnitID: @"<your adUnit Id>"
                                      rootViewController: self
                                                 adTypes: @[kGADAdLoaderAdTypeUnifiedNative]
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
            [trekNativeAdTableViewCell setGADUnifiedNativeAdData:_gADUnifiedNativeAd];
            return trekNativeAdTableViewCell;
        }
    }
    
    // For SuprAd
    if (indexPath.row == googleMediationSuprAdPosition) {
        if(_gADUnifiedSuprAd != nil) {
            TrekSuprAdTableViewCell *trekSuprAdTableViewCell = [tableView dequeueReusableCellWithIdentifier:@"TrekSuprAdTableViewCell" forIndexPath:indexPath];
            
            CGSize size = CGSizeMake(_viewWidth, _viewHeight);
            [trekSuprAdTableViewCell setGADUnifiedNativeAdData:_gADUnifiedSuprAd withViewSize:size];
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

#pragma mark : ScrlloView delegate

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
		if (_gADUnifiedSuprAd != nil) {
        [self.delegate rootViewControllerScrollViewDidScroll:scrollView];
    }
}

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

@end
```



If you ready to use SuprAd advertising type, please pay special attention to implement the content:

File: AotterTrekGADCustomEventNativeAd.m

CustomViewController need to modify your ViewController class.

```
@interface AotterTrekGADCustomEventNativeAd()<ViewControllerDelegate> {

		// 部分參數忽略
		.
		.
		.
		
    // CustomViewController 隨著自己定義的 ViewController 來決定
    // EX: SomeViewController (Your Custom ViewController)
    // declared: SomeViewController *_customViewController;
    ViewController *_customViewController;
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

