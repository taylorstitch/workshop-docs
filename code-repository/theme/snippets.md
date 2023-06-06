# snippets

**Product Meta Data**

The following snippet, when included on a Product page or within a Product loop, will set Liquid variables for rendering or performing logic on:

```html
{% raw %}{% include 'tsio-workshop-data-product', wsProduct: product %}{% endraw %}
```

Variables set up:

* `wsPhase` string where possible values are `crowd-sourced`, `pre-order`, `in-production`, `retail`
* `wsActive` boolean; true when Product phase is `crowd-sourced` or `pre-order`
* `wsStarts` date; of Workshop Project start
* `wsEnds` date; of Workshop Project end
* `wsTarget` number; representing sales target  
* `wsSales` number; representing current sales
* `wsProgress` number; percentage \(without `%`\) of wsSales/wsTarget
* `wsShipDates` array; of \(max 3\) potential ship dates

**Hiding Workshop Projects From "Regular" Collections**

Within a Collection loop, the logic below hides the `product-grid-item` include for all Workshop Projects until their phase is marked as "retail":

```html
{% raw %}
{% assign wsProductHide = false %}
{% include 'tsio-workshop-data-product', wsProduct: product %}
{% if wsPhase and wsPhase != 'retail' %}
  {% assign wsProductHide = true %}
{% endif -%}
{% unless wsProductHide %}
  {% include 'product-grid-item' %}
{% endunless %}
{% endraw %}
```