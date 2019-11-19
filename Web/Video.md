# Video AD

## 1. Put the code in where what you want.
```html
  <div data-trek-ad="video" data-place="placement_name"></div>
```


## 2. Execute this js code.
```js
AotterTrek('videoAd', {
    selector: '[data-trek-ad="video"]', // or passing element into it.
    onAdLoad: () => {
        // When ad load success then do something.
    },
    onAdFail: () => {
        // When ad load fail then do something.
    }
})
```

or another handy way to listen ad status callback:

```js
AotterTrek(function(API) {
    API.Event.on('onAdLoad', function(data) {
        if (data.place === 'placement_name') {
            // Do something.
        }
    });

    API.Event.on('onAdFail', function(data) {
        if (data.place === 'placement_name') {
            // Do something.
        }
    });
});
```


## 3. Your original `<div data-trek-ad="video" ...` will show up video ad.