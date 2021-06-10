# Banner Ad
## 建立版位
進入 [應用程式列表](https://trek.aotter.net/publisher/list/app) 的版位管理，建立版位名稱和類型

![Android](https://user-images.githubusercontent.com/48562635/121494446-55ed5400-ca0b-11eb-802b-fe90d2790603.jpg)

![iOS_BannerAd](https://user-images.githubusercontent.com/48562635/121494234-22122e80-ca0b-11eb-801d-41f8082ab76a.png)

## [GitHub 範例App](https://github.com/aotter/AotterTrek-Android-SDK/blob/master/android-sample/app/src/main/java/com/aotter/net/treksampleapp/activity/BannerAdListViewActivity.java)
```java
@BindView(R.id.listview)
ListView mListView;

@BindView(R.id.ad_banner)
TKMediaView mTkMediaView;

@BindView(R.id.ad_content_layout)
LinearLayout mAd_content_layout;

private TKAdN tkAdN;

// "banner" is custom payload place name
//  place name will show on Trek Consloe , please define it.
//  tkAdN = new TKAdN(this, "banner", "NATIVE"); if don't have category, you can without it.
tkAdN = new TKAdN(this, "banner", "category_name", "SUPR_AD");
tkAdN.setAdListener(new TKAdListener() {

  @Override
  public void onAdLoaded(TKAdNative nativeAd) {
    ...
  }

  @Override
  public void onAdError(TKError tkError) {
    ...
  }

  @Override
  public void onAdClicked(TKAdNative nativeAd) {
    ...
  }

  @Override
  public void onAdImpression(TKAdNative nativeAd) {
    ...
  }
});

```

```java
@Override
public void onAdLoaded(TKAdNative nativeAd) {

    tkAdN.registerAdView(activity, mAd_content_layout, mTkMediaView, nativeAd);
}
```

# Lifecycle

```java
@Override
public void onPause() {
  super.onPause();

  if (tkAdN != null) {
    tkAdN.pause();
  }
}

@Override
public void onResume() {
  super.onResume();

  if (tkAdN != null) {
    tkAdN.resume();
  }
}


@Override
public void onDestroy() {
  super.onDestroy();

  if (tkAdN != null) {
    tkAdN.destroy();
  }
}

```

### isExpired
check the ad is expired or not.
```java
nativeAd.isExpired();
```