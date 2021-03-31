# Banner Ad
## 建立版位
進入 [應用程式列表](https://trek.aotter.net/publisher/list/app) 的版位管理，建立版位名稱和類型

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F8ef1a669-a2fa-437a-8325-48d0b17a53a7.png)

![](https://tkmedia-cache.aotter.net/cache/https%3A%2F%2Ftkmedia.aotter.net%2Fmedia%2F325c4158-d797-4de9-9b7f-9450499b8223.png)

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