# TkAdn
---

> You can track `Conversion Rate` by using AotterTrek JS SDK, It's can track behavior while user browsing on Ad's target page.

## How it works?

1. While user click on an Ad, open new target page, the url will attached an `_tkadbc` track parameter, ex. `https://example.com/page/camp2017/?_tkadbc=fd91c867eff1b33bf5936d91058994...`
2. If the target page already include AotterTrek JS SDK, SDK will get parameter `_tkadbc` from url, and temporary store it in Cookie by `one` day.
3. User will heading different page at `same domain`, ex. `https://example.com/page/camp2017/step_two`, In timeliness, tacker code will keep as good.
4. Sending js event to AotterTrek Server by trigger an event by User (ex. submit an form or buying something)

> `Caution`: Please `DO NOT` install SDK on an `redirect-needed` page, because SDK depend on Url parameter, otherwise SDK will not working property.


#### Usage

##### 1. Please install SDK on every User-flow page.
##### 2. While user finished some action.
```js
AotterTrek('tkadn', '{event_name}')
```

> `{event_name}`  using it by any string to track what's step on. If event is include multi-step, AotterTrek will using `Conversion Funnel` to analyze it every progress.

---

#### Example
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>

  <form action="/form" onsubmit="reportEvent()">
    <input type="text" name="name" placeholder="你的名字">
    <button type="submit">送出</button>
  </form>
  <!-- Please check sdk was been installed -->
  <script>
  function reportEvent(){
    AotterTrek('tkadn', 'form_submit');
  }
  </script>
</body>
</html>
```
