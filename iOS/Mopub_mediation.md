# Mopub Medaiton Guide

## 1. add mediation file to your project

download mediation-files [here](https://github.com/aotter/AotterTrek-iOS-SDK/releases/download/3.3.5/Mopub-meidation-intergarion.zip)

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
   
- below is a sample named `RenderingViewTableViewCellForMopubNativeAd`. 
  
   - Note. You should implement this class by your own cause it depends on layout for your own app.

```objective-c
     //.h
     @interface RenderingViewTableViewCellForMopubNativeAd : UIView
     - (void)layoutSubviews;
     - (UILabel *)nativeMainTextLabel;
     - (UILabel *)nativeTitleTextLabel;
     - (UILabel *)nativeCallToActionTextLabel;
     - (UIImageView *)nativeIconImageView;
     - (UIImageView *)nativeMainImageView;
     - (UIImageView *)nativePrivacyInformationIconImageView;
     @end
     
     //.m
     @interface RenderingViewTableViewCellForMopubNativeAd()<MPNativeAdRendering>
       
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
     
     - (void)layoutSubviews{
         [super layoutSubviews];
         
         [self.labelTitle setTranslatesAutoresizingMaskIntoConstraints:NO];
         [self.labelSummary setTranslatesAutoresizingMaskIntoConstraints:NO];
         [self.labelInfo setTranslatesAutoresizingMaskIntoConstraints:NO];
         [self.iconImageView setTranslatesAutoresizingMaskIntoConstraints:NO];
         [self.imageViewBackground setTranslatesAutoresizingMaskIntoConstraints:NO];
         [self.seperator setTranslatesAutoresizingMaskIntoConstraints:NO];
         
         [self.labelTitle setFont:[UIFont systemFontOfSize:15]];
         [self.labelTitle setTextColor:[UIColor colorWithRed:63/255.0f
                                                       green:63/255.0f
                                                        blue:63/255.0f
                                                       alpha:1.0f]];
         [self.labelSummary setFont:[UIFont systemFontOfSize:12]];
         [self.labelSummary setTextColor:[UIColor colorWithRed:170/255.0f
                                                         green:170/255.0f
                                                          blue:170/255.0f
                                                         alpha:1.0f]];
         [self.labelInfo setFont:[UIFont systemFontOfSize:12]];
         [self.labelInfo setTextColor:[UIColor colorWithRed:150/255.0f
                                                      green:150/255.0f
                                                       blue:150/255.0f
                                                      alpha:1.0f]];
         [self.seperator setBackgroundColor:[UIColor colorWithRed:171/255.0f
                                                            green:171/255.0f
                                                             blue:171/255.0f
                                                            alpha:0.4f]];
         
         [self.labelTitle setNumberOfLines:2];
         [self.labelSummary setNumberOfLines:2];
         
         
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
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-0-[imageViewBackground]-0-|" options:0 metrics:nil views:views]];
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-3-[imageViewBackground]-3-|" options:0 metrics:nil views:views]];
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-9-[iconImageView(100)]" options:0 metrics:nil views:views]];
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-1-[iconImageView]-2-|" options:0 metrics:nil views:views]];
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:[iconImageView]-6-[labelTitle]-11-|" options:0 metrics:nil views:views]];
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-2-[labelTitle]" options:0 metrics:nil views:views]];
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:[labelTitle]-4-[labelSummary]-(>=2)-[labelInfo]-2-[seperator(1)]-0-|" options:NSLayoutFormatAlignAllLeading | NSLayoutFormatAlignAllTrailing metrics:nil views:views]];
         constraits = [constraits arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:[iconImageView]-(>=0)-[seperator]" options:0 metrics:nil views:views]];
         constraits = [constraits arrayByAddingObject:[NSLayoutConstraint constraintWithItem:self.iconImageView attribute:NSLayoutAttributeWidth relatedBy:NSLayoutRelationEqual toItem:self.iconImageView attribute:NSLayoutAttributeHeight multiplier:1.0f constant:0.0f]];
         
         [NSLayoutConstraint activateConstraints:constraits];
         
     }
     
     - (UILabel *)nativeMainTextLabel{
         return self.labelSummary;
     }
     
     - (UILabel *)nativeTitleTextLabel{
         return self.labelTitle;
     }
     
     - (UILabel *)nativeCallToActionTextLabel{
         return nil;
     }
     
     - (UIImageView *)nativeIconImageView{
         return self.iconImageView;
     }
     
     - (UIImageView *)nativeMainImageView{
         return nil;
     }
     
     - (UIImageView *)nativePrivacyInformationIconImageView{
         return nil;
     }
     
     -(void)layoutCustomAssetsWithProperties:(NSDictionary *)customProperties imageLoader:(MPNativeAdRenderingImageLoader *)imageLoader{
         
         NSLog(@"[RenderingView TableViewCell for MopubNativeAd] layoutCustomAssetsWithProperties: %@", customProperties);
         NSString *iconImage = customProperties[@"iconimage"];
         [imageLoader loadImageForURL:[NSURL URLWithString:iconImage] intoImageView:self.iconImageView];
     }
     @end
```

## 4. implement Mopub native ad

   - https://developers.mopub.com/publishers/ios/native/

## 5. Setup ad renderer

   - reference: https://developers.mopub.com/publishers/ios/integrate-networks/#native-ad-set-up-your-ad-renderers

```objective-c
MPStaticNativeAdRendererSettings *settings = [[MPStaticNativeAdRendererSettings alloc] init];
settings.renderingViewClass = [RenderingViewTableViewCellForMopubNativeAd class];
    
MPNativeAdRendererConfiguration *config_trek = [AotterTrekNativeAdRenderer rendererConfigurationWithRendererSettings:settings];
  
NSString *adUnitId = @"yourUnitAd";
MPNativeAdRequest *adRequest = [MPNativeAdRequest requestWithAdUnitIdentifier:adUnitId
                                                           rendererConfigurations:@[ config_trek]];

MPNativeAdRequestTargeting *targeting = [[MPNativeAdRequestTargeting alloc] init];
    targeting.desiredAssets = [NSSet setWithObjects:kAdTitleKey, kAdTextKey, kAdMainImageKey, kAdIconImageKey, kAdCTATextKey, nil];
    targeting.localExtras = @{@"ad category": self.category.name};
    
    adRequest.targeting = targeting;
```

## 6. Request ad and rendering view

   - reference : https://developers.mopub.com/publishers/ios/native/#requesting-and-displaying-native-ads
   - sample 

```objective-c

    [adRequest startWithCompletionHandler:^(MPNativeAdRequest *request, MPNativeAd *response, NSError *error) {
        self.mopubAd = response;
        UIView *nativeAdView = [response retrieveAdViewWithError:nil];
        if(nativeAdView){
            self.adView = [[UITableViewCell alloc] init];
            [self.adView.contentView addSubview:nativeAdView];
            
            [nativeAdView setTranslatesAutoresizingMaskIntoConstraints:NO];
            NSDictionary *views = @{@"nativeAdView": nativeAdView};
            NSArray *constraints = @[];
            constraints = [constraints arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"V:|-0-[nativeAdView]-0-|" options:0 metrics:nil views:views]];
            constraints = [constraints arrayByAddingObjectsFromArray:[NSLayoutConstraint constraintsWithVisualFormat:@"H:|-0-[nativeAdView]-0-|" options:0 metrics:nil views:views]];
            [NSLayoutConstraint activateConstraints:constraints];
            [self.mainTableView reloadData];
        }
    }];
```



