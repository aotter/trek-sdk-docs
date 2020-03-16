# AdManager (DoubleClick for Publisher)


### How to use AdManager publish AotterTrek Ads?
> This section will guide you through which how to publish AotterTrek Ads use AdManager, two steps you need to do, first is create `Ad Unit` and other is `Order`.

- [Before Started](/Web/AdManager?id=before-started)
- [Create Ad Unit](/Web/AdManager?id=create-ad-unit)
- [Create Order](/Web/AdManager?id=create-order)


### Before Started

> [!Danger]
> Currently, SuprAd only support for mobile and tablet.

> [!Danger]
> We're highly recommend that use **`320x180 (Mobile)`** or **`480x270 (Tablet or Desktop)`** for ad unit size, if you want to use custom setting, please follow `16:9` aspect ratio. Otherwise, it might be worked but not good for displaying Supr Ad.

> [!Danger]
> When you choose a size for your ad, you need to choose same size in next follow instructions.

### Create Ad Unit

1. In AdManager dashboard click `Inventory` > `Ad units` from sidebar.
2. Then, click `New ad unit` button.

![](/imgs/admanager/unit-1.png)

3. Will appear `New ad unit` section.
4. Fill up ad unit information, `name` and `size` are required, choose a size from dropdown, if there are hasn't your wanted option, you can typing in custom size in that `size` field.

> [!Tip]
> **`320x180 (Mobile)`** or **`480x270 (Tablet or Desktop)`**

![](/imgs/admanager/unit-2.png)

5. Then, click `SAVE` button.

![](/imgs/admanager/unit-3.png)

6. Find your new ad unit in `Ad units` list, and click it `name` to edit.
> Notice: Your new ad unit might be filtered by`filter` option.

![](/imgs/admanager/unit-4.png)

7. In edit ad unit section, click `Tags` tab, then click `CONTINUE` button.

![](/imgs/admanager/unit-5.png)

8. Next, keep click `CONTINUE` button.

![](/imgs/admanager/unit-6.png)

9. Final step, follow instructions on page, copy two separate code in your website `header` or `body (in where what you want to place)`.


### Create Order

1. In AdManager dashboard click `Delivery` > `Orders` from sidebar.
2. Then, click `New order` button.

![](/imgs/admanager/order-1.png)

3. Will appear `New order` section.
4. `name` and `advertiser` are required, if `advertiser` was empty, you can choose `Add a new company` from dropdown to create a new `advertiser`, then click `ADD LINE ITEM` button.

![](/imgs/admanager/order-2.png)

3. Will start create `New line item`, please select `Display` type, and next.

![](/imgs/admanager/order-3.png)

4. `name` and `Expected creatives` are required, `Expected creatives` please select ad unit size which previous we created, click `SAVE`.

![](/imgs/admanager/order-4.png)

5. Back to `Orders` list, find our new `Orders`, and click it, you will see `Line item` we created before.

![](/imgs/admanager/order-5.png)
![](/imgs/admanager/order-6.png)

6. Click it, start edit `Line item`, choose `Creatives` tab.

![](/imgs/admanager/order-7.png)

7. Then click `ADD CREATIVE` > `New creative` > `[ADUNITSIZE]`

![](/imgs/admanager/order-8.png)

8. Select `Third Party` type

![](/imgs/admanager/order-9.png)

9. Paste Aotter Trek js code and html tag in the `text-area`, and click `SAVE`.

> ref: [Getting Started](/Web/GettingStarted)

![](/imgs/admanager/order-10.png)

##### Example code:
> [!Warning]
> Change `placement_name` and `client id` values to your own.

> [!Warning]
> `placement_name` DO NOT USE Chinese character, use English instead.

```html
    <div data-trek-ad="suprad" data-place="placement_name"></div>

    <!-- start: trek sdk -->
    <script>
        (function(w, d, s, src, n) {
            var js, ajs = d.getElementsByTagName(s)[0];
            if (d.getElementById(n)) return;
            js = d.createElement(s); js.id = n;
            w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
            js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
        })(window, document, 'script', 'https://static.aottercdn.com/trek/sdk/3.2.7/sdk.js', 'AotterTrek');

        // Notice: replace your own client id.
        AotterTrek('init', 'yEFcFoJaruNorh5RqtuR');

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
    <!-- end: trek sdk -->


```

10. If you want to see that how ad look likes, you can back to `Creatives` list, and edit `Creative`, choose `Preview` tab to preview ad.

![](/imgs/admanager/order-11.png)