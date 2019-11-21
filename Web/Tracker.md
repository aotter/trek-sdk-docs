透過 `AotterTrek('init', 'CLIENT_ID')` 分析當前網頁的資訊。

````html
<!-- sdk -->
<script>
  (function(w, d, s, src, n) {
    var js, ajs = d.getElementsByTagName(s)[0];
    if (d.getElementById(n)) return;
    js = d.createElement(s); js.id = n;
    w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
    js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
  })(window, document, 'script', 'https://tkportal.aotter.net/public/3.0.0/sdk.js', 'AotterTrek');
  AotterTrek('init', 'CLIENT_ID');
</script>
```

送出更多事件
````html
<!-- sdk -->
<script>
  AotterTrek('send');
</script>
```