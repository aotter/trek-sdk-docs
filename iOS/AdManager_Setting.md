# AdManager setting

## Enable Ad cache

1. (default) when ad cache is eanbled, requesting ad will get failed immediately at the first time.
2. If you disable ad cache, all ad request will take some time to get callback.

```objective-c
//enable cache pool (by default)
[[TKAdManager sharedAPI] enableAdCachePoolWithIndiviaulPoolSize:<1~10>];

//disable cache pool
[[TKAdManager sharedAPI] disableAdCachePool];
```



## Ad pre-fetching

1. Only availble when ad cache eanbled.
2. Prefetch size depends on ad cach pool size, which can set by `enableAdCachePoolWithIndiviaulPoolSize`

```objective-c
//prefetch ad for TKAdNative
[[TKAdManager sharedAPI] prefetchNativeAdForPlace:@"<myPlace>"];

//or
[[TKAdManager sharedAPI] prefetchNativeAdForPlace:@"<myPlace>" category:@"category"];
```

```objective-c
//prefetch ad for Supr Ad
[[TKAdManager sharedAPI] prefetchSuprAdForPlace:@"<myPlace>"];

//or
[[TKAdManager sharedAPI] prefetchSuprAdForPlace:@"<myPlace>" category:@"category"];
```



## Clear Ad cache

1. Remove cached ad from storage if needed.

```objective-c
[[TKAdManager sharedAPI] clearAllAdCache];

[[TKAdManager sharedAPI] clearCacheForNativeAdWithPlace:@"<place>"];

[[TKAdManager sharedAPI] clearCahceForSuprAdWithPlace:@"<place>"];
```