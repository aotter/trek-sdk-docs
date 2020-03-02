# Supr Ad

##### 1. Install SDK first.

ref: [Installation](/Web/Install_SDK)

##### 2. Put the code in where what you want.
```html
  <div data-trek-ad="suprad" data-place="placement_name"></div>
```


> [!Warning]
> `placement_name` DO NOT USE Chinese character, use English instead.

##### 3. Execute this js code.
```html
<script>
AotterTrek('suprAd', {
    selector: '[data-trek-ad="suprad"]', // or passing element into it.
    onAdLoad: () => {
        // When ad load success then do something.
    },
    onAdFail: () => {
        // When ad load fail then do something.
    }
})
</script>
```

or another handy way to listen ad status callback:

```html
<script>
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
</script>
```


##### 4. Your original `<div data-trek-ad="suprad" ...` will transformed to SuprAd.