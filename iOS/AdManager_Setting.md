# AdManager setting

1. prefetch ad (optional) 非必要，一般fetchAd時就有此機制，但是提供以下方法，可在fetchAd之前就先預載

```objective-c
[[TKAdManager sharedAPI] prefetchAdForPlace:@"<myPlace>"];
```

1. prefetch ad for video (optional)

```objective-c
[[TKAdManager sharedAPI] prefetchAdVideoForPlace:@"<myPlace>"];
```

1. enable/disable cache (default is enabled)

```objective-c
//enable cache pool (by default)
[[TKAdManager sharedAPI] enableAdCachePoolWithIndiviaulPoolSize:<1~10>];

//disable cache pool
[[TKAdManager sharedAPI] disableAdCachePool];
```

1. clear the ad cache (optional)

```objective-c
[[TKAdManager sharedAPI] clearAllAdCache];

[[TKAdManager sharedAPI] clearCacheForNativeAdWithPlace:@"<place>"];

[[TKAdManager sharedAPI] clearCahceForSuprAdWithPlace:@"<place>"];
```