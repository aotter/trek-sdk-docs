# Supr Ad

```xml
<LinearLayout
    android:id="@+id/ad_container"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="10dp">

    <com.aotter.net.trek.ads.view.TKMediaView
        android:id="@+id/ad_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
    />
</LinearLayout>
```


```java
@Bind(R.id.ad_container)
LinearLayout mAdContainer;

@BindView(R.id.ad_view)
TKMediaView mTKMediaView;

private TKAdN tkAdN;

// "post_third" is custom payload place name
//  place name will show on Trek Consloe , please define it.
//  tkAdN = new TKAdN(this, "post_third", "SUPR_AD"); if don't have category, you can without it.
tkAdN = new TKAdN(this, "post_third", "category_name", "SUPR_AD");
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
public void onAdLoaded(NativeAd nativeAd) {
  ...

  tkAdN.registerAdView(activity, mAdContainer, mTKMediaView, nativeAd);
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

### isVideoAd
check the ad is video or not.
```java
nativeAd.isVideoAd();
```