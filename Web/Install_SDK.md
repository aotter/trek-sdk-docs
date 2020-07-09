# Installation
---

Place the following `<script>` near the end of your pages, right before the closing </body> tag, to enable them. 

> [!Warning]
> Change `CLIENT_ID` value to your own.

```html
<!-- start: trek sdk -->
<script>
  (function(w, d, s, src, n) {
    var js, ajs = d.getElementsByTagName(s)[0];
    if (d.getElementById(n)) return;
    js = d.createElement(s); js.id = n;
    w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
    js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
  })(window, document, 'script', 'https://static.aottercdn.com/trek/sdk/3.2.9/sdk.js', 'AotterTrek');

  AotterTrek('init', 'CLIENT_ID');

</script>
<!-- end: trek sdk -->
```