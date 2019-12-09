# APIs

## AotterTrek('init', 'CLIENT_ID')

> Initialize AotterTrek SDK, launch tracker code(_tkadbc), Ads at the same time.

#### Arguments
| order | name      | type     | note                                         |
|-------|-----------|----------|----------------------------------------------|
| 1     | init      | `string` | Only accept "nativeAd", "videoAd", "suprAd". |
| 2     | CLIENT_ID | `string` | A client_id from trek.                       |


#### Example
```js
AotterTrek('init', 'CLIENT_ID');
```
---

### AotterTrek(ADTYPE, OPTIONS)

> Send http request to get Ad response, inject response data to specific element.

> ref: [VideoAd](/Web/Video),[SuprAd](/Web/SuprAd)
<!-- > ref: [NativeAd](/Web/NativeAd),[VideoAd](/Web/Video),[SuprAd](/Web/SuprAd) -->

#### Arguments
| order | name    | type     | note                                         |
|-------|---------|----------|----------------------------------------------|
| 1     | ADTYPE  | `string` | Only accept "nativeAd", "videoAd", "suprAd". |
| 2     | OPTIONS | `object` | Below Options.                               |


> You can use html attribute to set options etc. `data-trek-network`, `data-place` ...

#### Options
| name        | type                  | note                                                                       |
|-------------|-----------------------|----------------------------------------------------------------------------|
| selector    | `string`or`element`   | The target element or selector.                                            |
| layout      | `string`              | If this set, using default Trek ad layout.                                 |
| place       | `string`              | Custom placement name, you can check out analytics data in Trek Dashboard. |
| trekNetwork | `string`              | Set ad network "trek" or "house", default by "trek".                       |
| onAdLoad    | `function`            | Trigger when ad success loaded.                                            |
| onAdFail    | `function`            | Trigger when ad fail loaded.                                               |


#### Example

```js
AotterTrek('nativeAd', {
	selector: '#native-ad',
	place: 'SIDEBAR_NATIVE_AD',
	layout: 'prebuilt',
	onAdLoad: function(node) {
		// Do something.
	},
	onAdFail: function(node) {
		// Do something.
	}
})
```

```js
AotterTrek('videoAd', {
	selector: '#video-ad',
	place: 'TOP_VIDEO_AD',
	trekNetwork: 'house',
	onAdLoad: function(node) {
		// Do something.
	},
	onAdFail: function(node) {
		// Do something.
	}
})
```

---

### AotterTrek('setUser', USER)
> Set user data

> ref: [User Setting](/Web/UserSetting)

#### Arguments
| order | name    | type     | note       |
|-------|---------|----------|------------|
| 1     | setUser | `string` |            |
| 2     | USER    | `object` | Below User.|


#### User
| name     | type     | note                                              |
|----------|----------|---------------------------------------------------|
| email    | `string` | User's email.                                     |
| birthday | `string` | User's birthday, only accept `yyyy/MM/dd` format. |
| fbId     | `string` | User's facebook id.                               |
| gender   | `string` | User's gender.                                    |


#### Example
```js
AotterTrek('setUser', {
  email: 'example@aotter.net',  
  birthday: '1999/01/01',
  fbId: '[USER_FACEBOOK_ID]',
  gender: 'MALE'
});

AotterTrek('init', 'CLIENT_ID');

AotterTrek('nativeAd', {
  selector: '#native-ad'
});
```

---


###  AotterTrek('tkadn', EVENT);

> Send trekadn event.

> ref: [TkAdn](/Web/TkAdn)

#### Arguments
| order | name    | type     | note             |
|-------|---------|----------|------------------|
| 1     | tkadn   | `string` |                  |
| 2     | EVENT   | `string` | Your event name. |


#### Example

```js
function reportEvent(){
	AotterTrek('tkadn', 'form_submit');
}
```

---


###  AotterTrek('send');

> Send an event for trek to analytics page information.

> ref: [Tracker](/Web/Tracker)

#### Example

```js
AotterTrek('send');
```

---

