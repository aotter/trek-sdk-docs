
### AotterTrek('init', 'CLIENT_ID')
初始化 AotterTrek，同時啟動追蹤碼、廣告。


#### example
```js
AotterTrek('init', 'CLIENT_ID');
```

### AotterTrek('nativeAd', [options = {}])
發送 http request 取得廣告資料，並在指定的元素上填入相對應的廣告資料。
#### arguments
1. **[options = {}]** *( Object )*: 參數設定
2. **[options.selector]** *( String | Element )*: The target element or selector
3. **[options.layout = "prebuilt"]** *( String )*: 直接使用 *AotterTrek* 的預設廣告版型。
4. **[options.place]** *( String )*: 自定版位名稱，可以在後台檢視此版位的成效。
5. **[options.onAdLoad]** *( Function(node) )*: 廣告載入成功時呼叫。
6. **[options.onAdFail]** *( Function(node) )*: 廣告載入失敗時呼叫。


#### example

```js
AotterTrek('nativeAd', {
	selector: '#native-ad',
	place: 'SIDEBAR_NATIVE_AD',
	layout: 'prebuilt',
	onAdLoad: function(node) {
		//$(node).show();
	},
	onAdFail: function(node) {
		//$(node).hide();
	}
})
```
### AotterTrek('setUser', user)
設定使用者資料。

#### arguments
1. **user** *( Object )*: 使用者的資料
2. **[user.email]** *( String )*: 使用者的Email信箱
3. **[user.birthday]** *( String )*: 使用者的生日，僅接受 yyyy/MM/dd 格式
4. **[user.fbId]** *( String )*: 使用者的facebook id
5. **[user.gender]** *( String )*: 使用者的性別

#### example
```js
AotterTrek('setUser', {
  email: 'example@aotter.net',  
  birthday '1999/01/01',
  fbId: '[USER_FACEBOOK_ID]',
  gender: 'MALE'
});

AotterTrek('init', 'CLIENT_ID');

AotterTrek('nativeAd', {
  selector: '#native-ad'
});
```
