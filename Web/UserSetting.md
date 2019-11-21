
# User Setting

> 

若想要提高使用者點擊廣告的動機，可以透過傳送使用者資料，讓廣告系統只傳送這名使用者偏好的廣告類型。

#### example
```js
AotterTrek('setUser', {
  email: 'example@aotter.net',  
  birthday '1999/01/01',
  fbId: '[USER_FACEBOOK_ID]',
  gender: 'MALE'  
});

AotterTrek('nativeAd', {
  selector: '#native-ad'
})
```
