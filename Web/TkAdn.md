## Summary
透過 AotterTrek javascript SDK ，將可追蹤使用者開啟廣告頁面之後的一連串事件行為，藉以分析廣告轉換效率。其原理與流程如下：

1. 使用者點按廣告時，開啟目標頁面之網址，其加掛的網址參數中，將會被AotterTrek廣告系統額加上一個名為`_tkadbc`追蹤碼參數，例如 `https://example.com/page/camp2017/?_tkadbc=fd91c867eff1b33bf5936d91058994...`
2. 該目標頁面的原始碼中引入有AotterTrek javascript SDK，本SDK將會自動抓取網址列上的 `_tkadbc` 參數，並暫存在cookie中，並有一日的時效
3. 使用者可以在「相同網域」下移動到其他頁面，例如 `https://example.com/page/camp2017/step_two`，只要在時效內，追蹤碼皆會被保留下來。
4. 在使用者觸發事件時（例如填寫並發送問券、購買商品），發送 js event回報給 AotterTrek Server

> 注意事項：由於我們依賴網址上的追蹤碼進行轉換追蹤，請勿將 AotterTrek SDK 放置在「經過網址跳轉後才能抵達的頁面」，否則將導致 `_tkadbc` 參數遺失，而在AotterTrek SDK啟動時偵測不到該追蹤碼，導致失效。


## Quick Start

請在每個流程頁面的`<head></head>`中置入以下的初始化程式碼
```html
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


2. 當使用者完成事件的時候，呼叫
```js
AotterTrek('tkadn', '{event_name}')
```
上述的 `{event_name}` 可以是開發者自行決定的任意字串，用來追蹤使用者走到哪一個步驟。若事件由多個步驟組成，AotterTrek將會進行各階段的轉換漏斗分析。

## 範例 ##
以下舉例為一個簡單的問券頁面，並在使用者填寫問券送出時發送事件，回報轉換發生：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>問券調查</title>
  <!-- Aotter Trek SDK -->
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
</head>
<body>

  <form action="/form" onsubmit="reportEvent()">
    <input type="text" name="name" placeholder="你的名字">
    <button type="submit">送出</button>
  </form>

  <script>
  function reportEvent(){
    AotterTrek('tkadn', 'form_submit');
  }
  </script>
</body>
</html>

```
