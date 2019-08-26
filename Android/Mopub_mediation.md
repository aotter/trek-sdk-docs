# Mopub Medaiton Guide
## Integrating MoPub SDK
Follow the instructions on the MoPub Site for setting up MoPub SDK.<br>
 https://developers.mopub.com/publishers/android/get-started/

## Integrated MoPub native ads
In order to mediate native ads, you must have already [integrated MoPub native ads](https://developers.mopub.com/publishers/android/native/).

----------
## Native Third-Party Integration Guide
### 1. Setup a Third-Party Network in your MoPub Account

AotterTrek Native Ads should be set up as custom native networks in your MoPub account. <br>
MoPub Support Site: https://developers.mopub.com/publishers/android/integrate-networks/

Please set up AotterTrek Native Ads as follows:

|  Type   | Custom Event Class         | Custom Event Class Data     |
| ----------- | ------------ | ---------- | 
| Native Ad    | com.mopub.nativeads.TrekNative       |   {"place_name":"< enter your place name here >"}    | 
|Supr Ad    | com.mopub.nativeads.TrekNative       |   {"place_name":"< enter your place name here >,"adType":"SUPR_AD""}    | 
 

![](https://lh3.googleusercontent.com/-G7ybK9sqeic/WEaIpN4ALzI/AAAAAAAAAk4/tj8p6QmnC6goZV-BPmHHsQq920h0QQiMACKgB/s0/trek.png)

### 2. Add the custom event files to your project

Add the folder ‘nativeads’ into com.mopub under your app’s src/ directory and copy the adapters from [檔案下載](https://trek.aotter.net/publisher/show/sdk?platform=ANDROID#home) that you want to include into the folder.

### 3. Install AotterTrek SDK

Follow the instructions on the [Initialize SDK](https://trek.aotter.net/publisher/show/sdk?platform=ANDROID#doc_1) for setting up AotterTrek SDK.
### 4. Use Mediation with category
```java
Map<String, Object> localExtras = new HashMap<>();
localExtras.put(TREK_AD_CATEGORY, "NBA");
TrekNativeAdSource adSource = new TrekNativeAdSource();
adSource.setLocalExtras(localExtras);
// Create an ad adapter that gets its positioning information from the MoPub Ad Server.
// This adapter will be used in place of the original adapter for the ListView.
mAdAdapter = new TrekMoPubAdAdapter((Activity) mContext, mAdapter, new MoPubServerPositioning(), adSource);
```
### 5. Use Mediation Supr Ad
```java
// Trek Audience Network
final TrekAdRenderer trekAdRenderer = new TrekAdRenderer(getActivity(),
        new ViewBinder.Builder(R.layout.native_ad_list)
                .titleId(R.id.ad_title)
                .mainImageId(R.id.postImg)
                .textId(R.id.ad_summary)
                .callToActionId(R.id.post_button)
                .addExtra("sponsor", R.id.ad_publisher)
                .addExtra("mediaView", R.id.ad_mediaview) //TKMediaView
                .build());

mAdAdapter.registerAdRenderer(trekAdRenderer);
```
MoPub Support Site: https://developers.mopub.com/publishers/android/integrate-networks/