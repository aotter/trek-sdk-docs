# SDK 3.4.0-beta1 integrate guide

--- 因應 iOS 14 的 AppTrackingTransparency 實作

## 注意！AotterTrek-iOS-SDK 3.4.0-beta1 

1. 只支援 Xcode 12 以上版本
2. 只支援手動安裝，不支援 cocoapods 安裝

## 安裝流程

### Step1. 拖拉 SDK 進入專案

### Step2. 為您的專案手動加入 dependency
- SystemConfiguration
- CoreMedia
- WebKit
- CoreTelephony
- AdSupport
- AVKit
- AVFoundation
- Foundation
- UIKit
- GoogleInteractiveMediaAds
- OMSDK_Aotternet
- AppTrackingTransparency

### Step3. 重要！在 info.plist 中加入 NSUserTrackingUsageDescription 字串
就像是要使用相機、照相機、定位等權限一樣，需要在 info.plist 中加入指定敘述
```
//info.plist
<key>NSUserTrackingUsageDescription</key>
<string>enable Tracking to get your IDFA.</string>
```

