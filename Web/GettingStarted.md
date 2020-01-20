# Getting Started
---

### 1. Installation

Place the following `<script>` near the end of your pages, right before the closing </body> tag.
Be aware to replace `YOUR_CLIENT_ID` to your client id or use our test id `yEFcFoJaruNorh5RqtuR`.

```html
<!-- start: trek sdk -->
<script>
  (function(w, d, s, src, n) {
    var js, ajs = d.getElementsByTagName(s)[0];
    if (d.getElementById(n)) return;
    js = d.createElement(s); js.id = n;
    w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
    js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
  })(window, document, 'script', 'https://static.aottercdn.com/trek/sdk/3.2.5/sdk.js', 'AotterTrek');

  // Notice: replace your own client id.
  AotterTrek('init', 'yEFcFoJaruNorh5RqtuR');

</script>
<!-- end: trek sdk -->
```

### 2. Execute AotterTrek()

Performing a `suprAd`, pass Ad type and options to Aotter Trek.

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

> ref: [Options](/Web/APIs#Options)

### 3. Insert `<div></div>` tag with attribute
 
Place Ad in where what you want.

```html
  <div data-trek-ad="suprad" data-place="placement_name"></div>
```

### 4. Done!
Aotter trek web sdk will parse `data-trek-ad="suprad"` to Ad.

#### Result:
![](/imgs/suprad.png)

### Full example

```html
<body>
    <div data-trek-ad="suprad" data-place="placement_name"></div>

    <!-- start: trek sdk -->
    <script>
        (function(w, d, s, src, n) {
            var js, ajs = d.getElementsByTagName(s)[0];
            if (d.getElementById(n)) return;
            js = d.createElement(s); js.id = n;
            w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
            js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
        })(window, document, 'script', 'https://static.aottercdn.com/trek/sdk/3.2.5/sdk.js', 'AotterTrek');

        // Notice: replace your own client id.
        AotterTrek('init', 'yEFcFoJaruNorh5RqtuR');
    </script>
    <!-- end: trek sdk -->

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

</body>

```