# Customize

How it works?

> SDK will detect `data-trek` attribute and finding same `prop` to binding data into element.

| selector                        	| data-binding                 	|
|---------------------------------	|------------------------------	|
| `<any data-trek="[PROP]"><any>` 	| `element.innerText = [PROP]` 	|
| `<img data-trek="[PROP]" />`    	| `img.src = [PROP]`           	|
| `<a data-trek="[PROP]"></a>`    	| `a.href = [PROP]`            	|

## Usage

> html example:

```html
<a id="native-ad" data-trek="URL">
  <img data-trek="IMG_ICON" />
  <h4 data-trek="TITLE"></h4>
  <h4 data-trek="TEXT"></h4>
</a>
<script>
  AotterTrek('nativeAd', {
    selector: '#native-ad'
  });
</script>
```

> And ad data will response:

```json
{
  "TITLE": "DEMO SOME TITLE",
  "TEXT": "DEMO SOME TEXT",   
  "IMG_ICON": "https://placehold.it/82x82",
  "URL": "https://trek.aotter.net/ad_url"
}
```

> Ad data will binding on your element.

```html
<a id="native-ad" data-trek="URL" href="https://trek.aotter.net/ad_url" data-trek-id="1">
  <img data-trek="IMG_ICON" src="https://placehold.it/82x82"/>
  <h4 data-trek="TITLE">DEMO SOME TITLE</h4>
  <h4 data-trek="TEXT">DEMO SOME TEXT</h4>
</a>
<script>
  AotterTrek('nativeAd', {
    selector: '#native-ad'
  });
</script>
```


---

## How to make sure ad will fully loaded then show ad.

> If success loaded ad, SDK will add `data-trek-id` attribute on your parent element ('selector selected element') after execute `AotterTrek.render('#native-ad')`

#### Examples

```html
<a id="native-ad" data-trek="URL">
  <!-- ... Ad contnet -->
</a>

<script>
  AotterTrek('nativeAd', {
    selector: '#native-ad'
  });
</script>
```

> Will transformed to:

```html
<a id="native-ad" data-trek="URL" href="https://trek.aotter.net/ad_url" data-trek-id="1">
  <!-- ... Ad content -->
</a>
<script>
  AotterTrek('nativeAd', {
    selector: '#native-ad'
  });
</script>
```
---

##### So, in this ability, you can use CSS to hide unloaded ad.

#### Example 1 (Using CSS):
```html
<style>
  #native-ad:not([data-trek-id]) {
    display:none;
  }
</style>
<a id="native-ad" data-trek="URL">
  <!-- Ad content -->
</a>
<script>
  AotterTrek('nativeAd', {
    selector: '#native-ad'
  });
</script>
```

> You can also add some style on `#native-ad` etc.

```css
#nativeAd img {
  /* some styles. */
}

#nativeAd div {
  /* some styles. */
}
```

#### Example 2 (Using JS):
```html
<a id="native-ad" data-trek="URL" style="display:none;">
  <!-- Ad Content -->
</a>
<script>
  AotterTrek('nativeAd', {
    selector: '#native-ad',
    onAdLoad: function(node) {
      $(node).show();
    },
    onAdFail: function(node) {
      $(node).hide();
    }
  });
</script>
```

---
## Ad data responsive property:

| props           	| description             	| render type       	|
|-----------------	|-------------------------	|-------------------	|
| TITLE           	| Ad Title                	| element.innerText 	|
| TEXT            	| Ad text content            	| element.innerText 	|
| IMG_ICON        	| Ad image 82px x 82px      	| img.src           	|
| IMG_ICON_HD     	| Ad image 300px x 300px    	| img.src           	|
| IMG_ICON_MAIN   	| Ad image 1280px x 720px   	| img.src           	|
| URL             	| Ad link                	| a.href            	|
| ADVERTISER_NAME 	| Ad provider name              	| element.innerText 	|
| SPONSOR         	| Ad Sponsor text ex. SPONSOR    	| element.innerText 	|
| CALL_TO_ACTION  	| Call to action ex. Learn More 	| element.innerText 	|
