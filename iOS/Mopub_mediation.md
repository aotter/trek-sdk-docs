# Mopub Mediation Guide

## 1. add mediation file to your project

Support Mopub SDK 5.9.0 and below:
download mediation-files [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.6.2/MoPub.Mediation_5.9.0.zip)

Support Mopub SDK 5.10.0 and above: 
download mediation-files  [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.6.2/MoPub.Mediation_5.10.0.zip)

   - AotterTrekNativeAdapater

   - AotterTrekNativeCustomEvent

   - AotterTrekNativeAdRenderer


## 2. set custom event class in your mopub netwrok dashboard.
   - Custom event class : `AotterTrekNativeCustomEvent`
   - Custom event class data : {"place_name": "your place name", "adType": "adType"}
     - adType `NATIVE_SUPRAD` for TKAdSuprAd
     - adType `NATIVE` for TKAdNative

## 3. build your own rendering class for ad layout

   - refernce: https://developers.mopub.com/publishers/ios/native/#define-the-layout-of-your-ad
   
- below is a sample named `MopubNativeAdRenderingView`. 
  
   - Note. You should implement this class by your own cause it depends on layout for your own app.

```objective-c
//.h

@interface MopubNativeAdRenderingView : UIView
- (void)layoutSubviews;
- (UILabel *)nativeMainTextLabel;
- (UILabel *)nativeTitleTextLabel;
- (UILabel *)nativeCallToActionTextLabel;
- (UIImageView *)nativeIconImageView;
- (UIImageView *)nativeMainImageView;
- (UIImageView *)nativePrivacyInformationIconImageView;
@end

  
//.m

#import "MPNativeAdRendering.h"
#import "MPNativeAdRenderingImageLoader.h"
  
@interface MopubNativeAdRenderingView()<MPNativeAdRendering>
@property (strong, nonatomic) UILabel *labelTitle;
@property (strong, nonatomic) UILabel *labelSummary;
@property (strong, nonatomic) UILabel *labelInfo;
@property (strong, nonatomic) UIImageView *iconImageView;
@property UIImageView *imageViewBackground;
@property UIView *seperator;
@end
  
@implementation MopubNativeAdRenderingView

-(instancetype)initWithFrame:(CGRect)frame{
    self = [super initWithFrame:frame];
    if(self){
        [self initialUI];
    }
    
    return self;
}

-(instancetype)initWithCoder:(NSCoder *)aDecoder{
    self = [super initWithCoder:aDecoder];
    if(self){
        [self initialUI];
    }
    
    return self;
}

-(void)initialUI{
    self.imageViewBackground = [[UIImageView alloc] init];
    [self.imageViewBackground setImage:[UIImage imageNamed:@"box_02_news"]];
    self.seperator = [[UIView alloc] init];
    self.labelTitle = [[UILabel alloc] init];
    self.labelSummary = [[UILabel alloc] init];
    self.labelInfo = [[UILabel alloc] init];
    self.iconImageView = [[UIImageView alloc] init];
}

- (void)layoutSubviews
{
    [super layoutSubviews];
    
    [self.labelTitle setTranslatesAutoresizingMaskIntoConstraints:NO];
    [self.labelSummary setTranslatesAutoresizingMaskIntoConstraints:NO];
    [self.labelInfo setTranslatesAutoresizingMaskIntoConstraints:NO];
    [self.iconImageView setTranslatesAutoresizingMaskIntoConstraints:NO];
    [self.imageViewBackground setTranslatesAutoresizingMaskIntoConstraints:NO];
    [self.seperator setTranslatesAutoresizingMaskIntoConstraints:NO];
    
    [self.labelTitle setTextColor:[UIColor colorWithRed:63/255.0f
                                                  green:63/255.0f
                                                   blue:63/255.0f
                                                  alpha:1.0f]];
    [self.labelSummary setFont:[UIFont systemFontOfSize:12]];
    [self.labelSummary setTextColor:[UIColor colorWithRed:170/255.0f
                                                    green:170/255.0f
                                                     blue:170/255.0f
                                                    alpha:1.0f]];
    
    
    [self.labelInfo setTextColor:[UIColor colorWithRed:150/255.0f
                                                 green:150/255.0f
                                                  blue:150/255.0f
                                                 alpha:1.0f]];
    
    if(UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad) {
        [self.seperator setBackgroundColor:UIColor.whiteColor];
    }else {
        [self.seperator setBackgroundColor:[UIColor colorWithRed:171/255.0f
                                                           green:171/255.0f
                                                            blue:171/255.0f
                                                           alpha:0.4f]];
    }
    
    [self.labelTitle setNumberOfLines:2];
    [self.labelSummary setNumberOfLines:2];
    [self.imageViewBackground setBackgroundColor:UIColor.whiteColor];
        
    [self addSubview:self.imageViewBackground];
    [self addSubview:self.labelTitle];
    [self addSubview:self.labelSummary];
    [self addSubview:self.labelInfo];
    [self addSubview:self.iconImageView];
    [self addSubview:self.seperator];
    
    
    NSDictionary *views = @{@"labelTitle": self.labelTitle,
                            @"labelSummary": self.labelSummary,
                            @"labelInfo": self.labelInfo,
                            @"iconImageView": self.iconImageView,
                            @"imageViewBackground": self.imageViewBackground,
                            @"seperator": self.seperator};
    
    NSArray *constraits = @[];
    
    if(UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad) {
        constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-0-[imageViewBackground]-0-|" options:0 metrics:nil views:views]];
        constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-5-[iconImageView(100@500)]" options:0 metrics:nil views:views]];
    }else {
        constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-3-[imageViewBackground]-3-|" options:0 metrics:nil views:views]];
        constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-5-[iconImageView(100)]" options:0 metrics:nil views:views]];
    }
    
    constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-0-[imageViewBackground]-0-|" options:0 metrics:nil views:views]];
    constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-5-[iconImageView]-5-|" options:0 metrics:nil views:views]];
    constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:[iconImageView]-6-[labelTitle]-11-|" options:0 metrics:nil views:views]];
    constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-2-[labelTitle]" options:0 metrics:nil views:views]];
    constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:[labelTitle]-4-[labelSummary]-(>=2)-[labelInfo]-2-[seperator(1)]-0-|" options:NSLayoutFormatAlignAllLeading | NSLayoutFormatAlignAllTrailing metrics:nil views:views]];
    constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:[iconImageView]-(>=0)-[seperator]" options:0 metrics:nil views:views]];
    constraits = [constraits arrayByAddingObject:[NSLayoutConstraint constraintWithItem:self.iconImageView attribute:NSLayoutAttributeWidth relatedBy:NSLayoutRelationEqual toItem:self.iconImageView attribute:NSLayoutAttributeHeight multiplier:1.0f constant:0.0f]];
    
    [NSLayoutConstraint activateConstraints:constraits];
    
}

- (UILabel *)nativeMainTextLabel
{
    return self.labelSummary;
}

- (UILabel *)nativeTitleTextLabel
{
    return self.labelTitle;
}

- (UILabel *)nativeCallToActionTextLabel
{
    return nil;
}

- (UIImageView *)nativeIconImageView
{
    return self.iconImageView;
}

- (UIImageView *)nativeMainImageView
{
    return nil;
}

- (UIImageView *)nativePrivacyInformationIconImageView
{
    return nil;
}

-(void)layoutCustomAssetsWithProperties:(NSDictionary *)customProperties imageLoader:(MPNativeAdRenderingImageLoader *)imageLoader{
    
		NSLog(@"[RenderingView TableViewCell for MopubNativeAd] layoutCustomAssetsWithProperties: %@", 		customProperties);
    
  	//NSString *callToActionString = customProperties[@"ctatext"];	
    //NSString *iconImage = customProperties[@"img_icon"];        	// 82x82
  	//NSString *mainImage = customProperties[@"mainimage"];       	// 1200x628
    NSString *titleString = customProperties[@"title"];
    NSString *textString = customProperties[@"text"];
    NSString *iconHDImage  = customProperties[@"iconimage"];      	// 300x300
    NSString *advertiserName = customProperties[@"advertiserName"];
    NSString *sponsored = customProperties[@"sponser"];
    
    self.labelTitle.text = titleString;
    self.labelSummary.text = textString;
    if(advertiserName && [advertiserName length]>0){
        self.labelInfo.text = [NSString stringWithFormat:@"%@ | %@",sponsored,advertiserName];
    }
    else{
        self.labelInfo.text = [NSString stringWithFormat:@"%@",sponsored];
    }
    
    [imageLoader loadImageForURL:[NSURL URLWithString:iconHDImage] intoImageView:self.iconImageView];
    
}

@end


```

**If you have need SuprAd needs**，below is a sample named `MopubSuprAdRenderingView`. 

- Note. You should implement this class by your own cause it depends on layout for your own app.

  ```objective-c
  //.h
  
  @interface MopubSuprAdRenderingView : UIView
  - (UIImageView *)nativeMainImageView;
  @end
    
  //.m
    
  #import "MPNativeAdRendering.h"
  #import "MPNativeAdRenderingImageLoader.h"
  
  @interface MopubSuprAdRenderingView()<MPNativeAdRendering>
  @property (strong, nonatomic) UIImageView *mainImageView;
  @end
    
  @implementation MopubSuprAdRenderingView
  
  -(instancetype)initWithFrame:(CGRect)frame{
      self = [super initWithFrame:frame];
      if(self){
          [self initialUI];
      }
      
      return self;
  }
  
  -(instancetype)initWithCoder:(NSCoder *)aDecoder{
      self = [super initWithCoder:aDecoder];
      if(self){
          [self initialUI];
      }
      
      return self;
  }
  
  -(void)initialUI{
      self.mainImageView = [[UIImageView alloc] init];
  }
  
  - (UIImageView *)nativeMainImageView {
      return self.mainImageView;
  }
  
  -(void)layoutSubviews {
      [super layoutSubviews];
      
      [self addSubview:self.mainImageView];
      
      [self.mainImageView setTranslatesAutoresizingMaskIntoConstraints:NO];
      [self.mainImageView.leadingAnchor constraintEqualToAnchor:self.leadingAnchor].active = YES;
      [self.mainImageView.trailingAnchor constraintEqualToAnchor:self.trailingAnchor].active = YES;
      [self.mainImageView.topAnchor constraintEqualToAnchor:self.topAnchor].active = YES;
      [self.mainImageView.bottomAnchor constraintEqualToAnchor:self.bottomAnchor].active = YES;
  }
  
  @end
  ```

  

## 4. implement Mopub native ad

   - https://developers.mopub.com/publishers/ios/native-manual/

## 5. Setup ad renderer

   - reference: https://developers.mopub.com/publishers/ios/integrate-networks/#native-ad-set-up-your-ad-renderers

```objective-c
// .m

@interface ViewController () <UITableViewDataSource, UITableViewDelegate> {
  @property (strong, nonatomic) MPNativeAdRequest *adRequest;
}
.
.
.

@implementation ViewController

- (void)viewDidAppear:(BOOL)animated {
    [super viewDidAppear:animated];
    [self configuareMoPubNativeAd];
}

// configuare MoPub request
-(void) configuareMoPubNativeAd {
    
    NSString *nativeAdUnitId = @"your AdUnitId";
    
    MPStaticNativeAdRendererSettings *settings = [[MPStaticNativeAdRendererSettings alloc] init];
    settings.renderingViewClass = [MopubNativeAdRenderingView class];
    
    settings.viewSizeHandler = ^(CGFloat maximumWidth) {
        return CGSizeMake(maximumWidth, 103.0f);
    };
  
    MPNativeAdRendererConfiguration *config_trek = [AotterTrekNativeAdRenderer rendererConfigurationWithRendererSettings:settings];
  
    _adRequest = [MPNativeAdRequest requestWithAdUnitIdentifier:nativeAdUnitId
                                                           rendererConfigurations:@[config_trek]];
    

    MPNativeAdRequestTargeting *targeting = [MPNativeAdRequestTargeting targeting];
    targeting.desiredAssets = [NSSet setWithObjects:kAdTitleKey, kAdTextKey, kAdMainImageKey, kAdIconImageKey, kAdCTATextKey, nil];
  	targeting.localExtras = @{@"category": @"some category Name"};
    
    _adRequest.targeting = targeting;
  
  //See the Step 6: sendMoPubRequest method  
  [self sendMoPubRequest];
}
@end

```

## 6. Request ad and rendering view

   - reference : https://developers.mopub.com/publishers/ios/native/#requesting-and-displaying-native-ads
   - sample 

```objective-c
// .m 

// Send MoPub request
-(void) sendMoPubRequest {
  [_adRequest startWithCompletionHandler:^(MPNativeAdRequest *request, MPNativeAd *response, NSError *error) {
        
        MPNativeAd *mopubAd = response;
        UIView *nativeAdView = [response retrieveAdViewWithError:nil];
    
      if(nativeAdView){
          UITableViewCell *adView = [[UITableViewCell alloc] init];
          [adView.contentView addSubview:nativeAdView];
            
          [nativeAdView setTranslatesAutoresizingMaskIntoConstraints:NO];
          NSDictionary *views = @{@"nativeAdView": nativeAdView};
          NSArray *constraints = @[];
          constraints = [constraints arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-0-[nativeAdView]-0-|" options:0 metrics:nil views:views]];
          constraints = [constraints arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-0-[nativeAdView]-0-|" options:0 metrics:nil views:views]];
          [NSLayoutConstraint activateConstraints:constraints];
        
        // Refresh UI
        ["Your TableView or CollectionView" reloadData];
      }
    }];
}
```



## 7. Additional Method for SuprAd (AdScrolled)

**If you have need SuprAd needs**, the AotterTrek's SuprAd type ad need to be notified when the ad view is scrolled, in order to show some specfic view according to the position of the ad. Therefore, you should add additional method in following:

```objective-c
//.m

#pragma mark : ScrlloView delegate

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
		// The _mopubAd type is "MPNativeAd"
    if (_mopubAd != nil) {
      
        // The postNotificationName please fill in "SuprAdScrolled"
        [[NSNotificationCenter defaultCenter]postNotificationName:@"SuprAdScrolled"
                                                           object:self
                                                         userInfo:nil];
    }
}
```
