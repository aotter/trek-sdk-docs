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

this part depends on your app's life cycle, but we provide example at "sample ad view for rendering AotterTrek naitve ad" folder for you:

```
- (void)viewDidLoad {
    [super viewDidLoad];
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"Done" style:UIBarButtonItemStyleDone target:self action:@selector(done:)];
    [self initialTableView];
    
    /* some adLoader options may help
        GADNativeAdImageAdLoaderOptions
        GADNativeAdMediaAdLoaderOptions
        GADMultipleAdsAdLoaderOptions
     */
    
    
    
    //GAD adLoader
    self.adLoader = [[GADAdLoader alloc]
          initWithAdUnitID:@"<your adUnit Id>"
        rootViewController:self
                   adTypes:@[kGADAdLoaderAdTypeUnifiedNative]
                   options:@[  ]];
    self.adLoader.delegate = self;

    [self.adLoader loadRequest:[GADRequest request]];
}



-(void)viewWillDisappear:(BOOL)animated{
}

-(IBAction)done:(id)sender{
    [self.navigationController dismissViewControllerAnimated:YES completion:nil];
}


-(void)initialTableView{
    self.mainTableView = [[UITableView alloc] init];
    [self.view addSubview:self.mainTableView];
    self.mainTableView.delegate = self;
    self.mainTableView.dataSource = self;
    [self.mainTableView setTranslatesAutoresizingMaskIntoConstraints:NO];
    NSDictionary *views = @{@"tableView": self.mainTableView};
    NSArray *arr = @[];
    arr = [arr arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-0-[tableView]-0-|" options:0 metrics:nil views:views]];
    arr = [arr arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-0-[tableView]-0-|" options:0 metrics:nil views:views]];
    [NSLayoutConstraint activateConstraints:arr];
    [self.mainTableView registerClass:[UITableViewCell class] forCellReuseIdentifier:@"adCell"];
    [self.mainTableView registerClass:[UITableViewCell class] forCellReuseIdentifier:@"cell"];
    
    UIRefreshControl *refresh = [[UIRefreshControl alloc] init];
    [refresh addTarget:self action:@selector(onRefresh:) forControlEvents:UIControlEventValueChanged];
    [self.mainTableView addSubview:refresh];
}

-(void)onRefresh:(UIRefreshControl *)refreshControl{
    [refreshControl endRefreshing];
    self.adCell = nil;
    [self.mainTableView reloadData];

}


-(void)setAdView:(GADUnifiedNativeAdView *)adView{
    self.adCell = [self.mainTableView dequeueReusableCellWithIdentifier:@"adCell"];
    [self.adCell.contentView addSubview:adView];
    [adView setTranslatesAutoresizingMaskIntoConstraints:NO];
    [adView.leadingAnchor constraintEqualToAnchor:self.adCell.contentView.leadingAnchor].active = YES;
    [adView.trailingAnchor constraintEqualToAnchor:self.adCell.contentView.trailingAnchor].active = YES;
    [adView.topAnchor constraintEqualToAnchor:self.adCell.contentView.topAnchor].active = YES;
    [adView.bottomAnchor constraintEqualToAnchor:self.adCell.contentView.bottomAnchor].active = YES;
}


#pragma mark - GAD adLoader
- (void)adLoader:(GADAdLoader *)adLoader didReceiveUnifiedNativeAd:(GADUnifiedNativeAd *)nativeAd{
    NSLog(@"[GAD adLoader] didReceiveUnifiedNativeAd: %@", nativeAd);
    GoogleUnifiedNativeAdView *nativeAdView = [[NSBundle mainBundle] loadNibNamed:@"GoogleUnifiedNativeAdView" owner:nil options:nil].firstObject;
    [self setAdView:nativeAdView];
    
    // Set the mediaContent on the GADMediaView to populate it with available
    // video/image asset.
    nativeAdView.mediaView.mediaContent = nativeAd.mediaContent;
    
    // Populate the native ad view with the native ad assets.
    // The headline is guaranteed to be present in every native ad.
    ((UILabel *)nativeAdView.headlineView).text = nativeAd.headline;

    // These assets are not guaranteed to be present. Check that they are before
    // showing or hiding them.
    ((UILabel *)nativeAdView.bodyView).text = nativeAd.body;
    nativeAdView.bodyView.hidden = nativeAd.body ? NO : YES;

    [((UIButton *)nativeAdView.callToActionView)setTitle:nativeAd.callToAction
                                                forState:UIControlStateNormal];
    nativeAdView.callToActionView.hidden = nativeAd.callToAction ? NO : YES;

    if(nativeAd.icon.image){
        ((UIImageView *)nativeAdView.iconView).image = nativeAd.icon.image;
    }
    else if (nativeAd.icon.imageURL){
        [((UIImageView *)nativeAdView.iconView) sd_setImageWithURL:nativeAd.icon.imageURL];
    }
    nativeAdView.iconView.hidden = NO;


    ((UILabel *)nativeAdView.storeView).text = nativeAd.store;
    nativeAdView.storeView.hidden = nativeAd.store ? NO : YES;

    ((UILabel *)nativeAdView.priceView).text = nativeAd.price;
    nativeAdView.priceView.hidden = nativeAd.price ? NO : YES;

    ((UILabel *)nativeAdView.advertiserView).text = nativeAd.advertiser;
    nativeAdView.advertiserView.hidden = nativeAd.advertiser ? NO : YES;

    // In order for the SDK to process touch events properly, user interaction
    // should be disabled.
    nativeAdView.callToActionView.userInteractionEnabled = NO;

    // Associate the native ad view with the native ad object. This is
    // required to make the ad clickable.
    nativeAdView.nativeAd = nativeAd;
    
    [self.mainTableView reloadData];
}

- (void)adLoader:(nonnull GADAdLoader *)adLoader didFailToReceiveAdWithError:(nonnull GADRequestError *)error {
    NSLog(@"[GAD adLoader] error: %@", error.description);
}

- (void)nativeAdDidRecordImpression:(GADUnifiedNativeAd *)nativeAd {
    NSLog(@"[GAD adLoader] nativeAdDidRecordImpression");
}

- (void)nativeAdDidRecordClick:(GADUnifiedNativeAd *)nativeAd {
  NSLog(@"[GAD adLoader] nativeAdDidRecordClick");
}

- (void)nativeAdWillPresentScreen:(GADUnifiedNativeAd *)nativeAd {
  NSLog(@"[GAD adLoader] nativeAdWillPresentScreen");
}

- (void)nativeAdWillDismissScreen:(GADUnifiedNativeAd *)nativeAd {
  NSLog(@"[GAD adLoader] nativeAdWillDismissScreen");
}

- (void)nativeAdDidDismissScreen:(GADUnifiedNativeAd *)nativeAd {
  NSLog(@"[GAD adLoader] nativeAdDidDismissScreen");
}

- (void)nativeAdWillLeaveApplication:(GADUnifiedNativeAd *)nativeAd {
  // The native ad will cause the application to become inactive and
  // open a new application.
    NSLog(@"[GAD adLoader] nativeAdWillLeaveApplication");
}

#pragma mark -

#define adIndex 10

-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    if(indexPath.row == adIndex && self.adCell){
        return 320;
    }
    return 44;
}

- (nonnull UITableViewCell *)tableView:(nonnull UITableView *)tableView cellForRowAtIndexPath:(nonnull NSIndexPath *)indexPath {
    if(indexPath.row == adIndex && self.adCell){
        return self.adCell;
    }
    
    UITableViewCell *cell = [self.mainTableView dequeueReusableCellWithIdentifier:@"cell" forIndexPath:indexPath];
    cell.textLabel.text = [NSString stringWithFormat:@"index: %ld", indexPath.row];
    return cell;
}

- (NSInteger)tableView:(nonnull UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return 50;
}
```

---

## 實作: 串接 Google Mediation for NativeAd:

Download:  https://github.com/aotter/AotterTrek-iOS-SDK/releases/tag/3.3.5

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



Custom TableViewCell (or CollectionViewCell)，here use TableViewCell.

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

宣告一個設定 GADUnifiedNativeAd 方法，在外部調用:

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

![截圖 2020-12-16 下午3.59.44](/Users/justintsou/Desktop/截圖 2020-12-16 下午3.59.44.png)



File: ViewController.m (Can use your Custom ViewController)

```
// Define the display position of the ad in the TableView
static NSInteger googleMediationNativeAdPosition = 6;
.
.
.

@interface ViewController ()<GADUnifiedNativeAdLoaderDelegate, UITableViewDataSource, UITableViewDelegate> {
    GADUnifiedNativeAd *_gADUnifiedNativeAd;
}

@implementation ViewController

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
     
    if (indexPath.row == googleMediationNativeAdPosition) {
        if (_gADUnifiedNativeAd != nil) {
            TrekNativeAdTableViewCell *trekNativeAdTableViewCell = [tableView dequeueReusableCellWithIdentifier:@"TrekNativeAdTableViewCell" forIndexPath:indexPath];
            [trekNativeAdTableViewCell setGADUnifiedNativeAdData:_gADUnifiedNativeAd];
            return trekNativeAdTableViewCell;
        }
    }
    
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"Cell" forIndexPath:indexPath];
    cell.textLabel.text = [[NSString alloc]initWithFormat:@"index:%ld",(long)indexPath.row];
    return  cell;
}


#pragma mark - UITableViewDelegate

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {

    if (indexPath.row == googleMediationNativeAdPosition) {
    		// The number 80 can adjust it.
        return _gADUnifiedNativeAd == nil ? 0:80;
    }
    return 80;
}


#pragma mark - GADUnifiedNativeAdLoaderDelegate

- (void)adLoader:(nonnull GADAdLoader *)adLoader didReceiveUnifiedNativeAd:(nonnull GADUnifiedNativeAd *)nativeAd {
    
    // Delegate 回來的 nativeAd 已經可以接取到自己的 Custom Ad View，
    // 在這裡我將 nativeAd 放到 CustomTableViewCell 去接資料
    
    if (nativeAd != nil) {
        _gADUnifiedNativeAd = nativeAd;
        [self.adTableView reloadData];
    }
}

- (void)adLoader:(nonnull GADAdLoader *)adLoader didFailToReceiveAdWithError:(nonnull GADRequestError *)error {
    NSLog(@"Error Message:%@",error.description);
}

@end
```



## 實作: 串接 Google Mediation for SuprAd:

Download:  https://github.com/aotter/AotterTrek-iOS-SDK/releases/tag/3.3.5

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



Custom TableViewCell (or CollectionViewCell)，here use TableViewCell.

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

宣告一個設定 GADUnifiedNativeAd 方法，在外部調用:

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

![截圖 2020-12-16 下午4.07.40](/Users/justintsou/Desktop/截圖 2020-12-16 下午4.07.40.png)



File: ViewController.h (Can use your Custom ViewController)

"SuprAd" need rootViewController Scroll delegate

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

```
// Define the display position of the ad in the TableView
static NSInteger googleMediationSuprAdPosition = 2;
.
.
.

@interface ViewController ()<GADUnifiedNativeAdLoaderDelegate, UITableViewDataSource, UITableViewDelegate> {
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

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    if (indexPath.row == googleMediationSuprAdPosition) {
        return _gADUnifiedSuprAd == nil ? 0:_viewHeight;
    }
    
    return 80;
}

#pragma mark : Scroll delegate

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    [self.delegate rootViewControllerScrollViewDidScroll:scrollView];
}

#pragma mark - GADUnifiedNativeAdLoaderDelegate

- (void)adLoader:(nonnull GADAdLoader *)adLoader didReceiveUnifiedNativeAd:(nonnull GADUnifiedNativeAd *)nativeAd {
    
    // Delegate 回來的 nativeAd 已經可以接取到自己的 Custom Ad View，
    // 這部分可以將 nativeAd 放到 CustomTableViewCell 去接資料
    
    if (nativeAd != nil) {
        _gADUnifiedNativeAd = nativeAd;
        [self.adTableView reloadData];
    }
}

- (void)adLoader:(nonnull GADAdLoader *)adLoader didFailToReceiveAdWithError:(nonnull GADRequestError *)error {
    NSLog(@"Error Message:%@",error.description);
}

@end
```



需要特別注意: AotterTrekGADCustomEventNativeAd.m

CustomViewController need to modify your ViewController class.

```
@interface AotterTrekGADCustomEventNativeAd()<ViewControllerDelegate> {
    NSString *_adType;
    NSString *_adPlace;
    NSError *_jsonError;
    NSString *_errorDescription;
    NSDictionary *_jsonDic;
    TKAdSuprAd *_suprAd;
    TKAdNative *_adNatve;
    
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
    
    
    // Parse serverParameter
    
    if (serverParameter != nil && ![serverParameter isEqual: @""]) {
        NSData *data = [serverParameter dataUsingEncoding:NSUTF8StringEncoding];
        _jsonDic = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];
        
    }else {
        _errorDescription = @"You must add AotterTrek adType in Google AdMob CustomEvent protal.";
        
        NSDictionary *userInfo = @{NSLocalizedDescriptionKey : _errorDescription,
                                   NSLocalizedFailureReasonErrorKey : _errorDescription};
        
        NSError *error = [NSError errorWithDomain:customEventErrorDomain
                                             code:0
                                         userInfo:userInfo];
        
        [self.delegate customEventNativeAd:self didFailToLoadWithError:error];
        return;
    }
    
    if ([[_jsonDic allKeys] containsObject:@"adType"] && [[_jsonDic allKeys] containsObject:@"adPlace"] ) {
        
        _adType = [_jsonDic objectForKey:@"adType"];
        _adPlace = [_jsonDic objectForKey:@"adPlace"];

        if ([_adType isEqualToString:@"nativeAd"]) {
            [self fetchTKAdNativeWithAdPlace:_adPlace];
        }else if ([_adType isEqualToString:@"suprAd"]) {
            [self fetchTKSuprAdWithAdPlace:_adPlace WithRootViewController:rootViewController];
        }
    }
}


#pragma mark - ViewControllerDelegate (CustomViewController Scroll Delegate)

- (void)rootViewControllerScrollViewDidScroll:(UIScrollView *)scrollView {
    if (_suprAd != nil) {
        [_suprAd notifyAdScrolled];
    }
}

@end
```

