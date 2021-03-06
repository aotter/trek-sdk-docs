# Native Ad

## 建立版位

進入 [應用程式列表](https://trek.aotter.net/publisher/list/app) 的版位管理，建立版位名稱和類型

![Android](https://user-images.githubusercontent.com/48562635/121494446-55ed5400-ca0b-11eb-802b-fe90d2790603.jpg)

<img width="699" alt="nativead" src="https://user-images.githubusercontent.com/48562635/121494559-6f8e9b80-ca0b-11eb-953e-c9e95c14c1a6.png">

```java
@Bind(R.id.native_ad_container)
LinearLayout mNative_ad_container;

@Bind(R.id.native_ad_title)
TextView mNative_ad_title;

@Bind(R.id.native_ad_body)
TextView mNative_ad_body;

private TKAdN tkAdN;

// "post_third" is custom payload place name
//  place name will show on Trek Consloe , please define it.
//  tkAdN = new TKAdN(this, "post_third", "NATIVE"); if don't have category, you can without it.
tkAdN = new TKAdN(this, "post_third", "category_name", "NATIVE");
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

  String adTitle = nativeAd.getAdTitle();
  String adText = nativeAd.getAdText();
  String adActionText = nativeAd.getActionText();
  String adSponser = nativeAd.getAdSponsor();
  String adVertiserName = nativeAd.getAdAdvertiserName();
  String adimg_icon_url = nativeAd.getAdImgIcon(); //82x82
  String adimg_icon_hd_url = nativeAd.getAdImgIconHd(); //300x300
  String adimg_main_url = nativeAd.getAdImgMain(); //1200x627

  mNative_ad_title.setText(adTitle);
  mNative_ad_body.setText(adText);

  tkAdN.registerAdView(activity, mNative_ad_container, nativeAd);
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