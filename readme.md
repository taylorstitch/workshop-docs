# The Workshop

Crowd-Funding app for Shopify stores.  (beta)

## Install Procedure

### 1. Store Authentication

We are not yet public and so not listed in the Shopify App Store officially, although there is a submission in progress&hellip; For now stores can install via the following URL, substituting your permanent `myshopify` subdomain in for `{{ store }}` and copying into your favorite web browser:

``` html
https://tsio-workshop.herokuapp.com/?shop={{ store }}.myshopify.com
```

### 2. Webhook Install

The application receives `order.paid` and `order.refund` webhooks from your store so that it can track sales over time and progress towards your goals, in realtime. You can install these webhooks via the "Settings" tab within the app admin.

### 3. App Assets Install

The application will add various code snippets to your store's primary theme, for inclusion on Product & Collection templates. **It will not publish these to your theme's front-end**, it simply adds the code snippets and makes them available to the the theme files so that you can use the following Liquid to add related elements in any places you deem appropriate:

#### 3.1 Product Section

``` twig   
{% section 'tsio-workshop-product' %}
```

The purpose of this snippet is to display goal progress, a remaining time countdown [and estimated ship dates tbc].

##### Product Section Styling

Various aspect of this section — including the color(s), size and style of the progress bar can be altered via your theme setting panel, when viewing an active Project's Product that has the included workshop section. The rendered HTML structure of the included elements is also shown below, for the purposes of further customization styling beyond what is inherited from your theme (typography, font sizing, etc.):

``` twig
<section class="ws-data">
  <ul>
    <li class="ws-period">
      <span>Workshop ends:</span>
      <time>02/03/2019</time>
    </li>
    <li class="ws-progress">67% Funded</li>
  </ul>
  <div class="ws-estimated-ship-date ws-multiple-ship-dates">
    <p><b>Estimated Shipping (Batch One): <span>04/12 – 05/2</span></b></p>
    <p class="ws-cut-off"><i>If ordered after 02/9</i></p>
    <p><b>Estimated Shipping (Batch Two): <span>05/3 – 05/23</span></b></p>
  </div>
</section>
```

Some basic layout styles are applied but easily overridden by targeting the element classes specified above.

##### Workaround For Nesting Shopify Sections

Shopify does not allow nesting of Sections out-of-the-box, but there is a workaround, example as follows, assuming your product.liquid collection template contains a "product-template" include...

Instead of using the syntax above i.e. `{% section 'tsio-workshop-product' %}`, you should create a "token" e.g. `%%tsio-workshop-product%%`, then you can do something like this (also replacing the `shopify-section` class to avoid css bugs):

``` twig
{% capture wsProductSection %}
  {% section 'tsio-workshop-product' %}
{% endcapture %}
{% assign wsProductSection = wsProductSection |  replace: 'class="shopify-section"', 'class="shopify-section-nested"' %}
{% capture product_template %}{% section 'product-template' %}{% endcapture %}
{% assign product_template = product_template | replace: "%%tsio-workshop-product%%", wsProductSection %}
{{ product_template }}
```

#### 3.2 Other Theme Elements

##### Product Meta Data

The following snippet, when included on a Product page or within a Product loop, will set Liquid variables for rendering or performing logic on:

``` twig
{% include 'tsio-workshop-data-product', wsProduct: product %}
```

Variables set up:

* `wsPhase` string where possible values are `crowd-sourced`, `pre-order`, `in-production`, `retail`
* `wsActive` boolean; true when Product phase is `crowd-sourced` or `pre-order`
* `wsStarts` date; of Workshop Project start
* `wsEnds` date; of Workshop Project end
* `wsTarget` number; representing sales target  
* `wsSales` number; representing current sales
* `wsProgress` number; percentage (without `%`) of wsSales/wsTarget
* `wsShipDates` array; of (max 3) potential ship dates

##### Product Estimated Shipping Ranges

The following snippet, when included on a Product page or within a Product loop, will set Liquid variables for rendering or performing logic on estimated shipping dates and order cut-off deadlines.


Variables set up:

* `wsShipDatesCount` number; of ship dates entered (max 3)
* `wsOrderDeadlinesCount` number; of order cut-off dates entered (max 2)
* `wsShipDates` array; of ship dates
* `wsOrderDeadlines` array; of ship dates
* `estShipRange1` thru `estShipRange3` string(s); calculated and formatted range, extending two weeks from the dates specified
* `estShipRangeTotal` string; calculated and formatted range, from the first date specified and extending two weeks beyond the final date  
* `cutOffDate1` thru `cutOffDate2` string(s); formatted dates  

This snippet is included in the `tsio-workshop-product` Section as standard, along with `tsio-workshop-estimated-shipping-alert` that prints the above variables in CSS-styled HTML for display on Product pages. You may get mileage from the ship range snippet itself and/or the logic contained within elsewhere too e.g. in Cart, Transactional Emails, and Customer Account templates.

##### Product Line Item Property Field

A Product's Workshop Project data, including its current phase (e.g. 'crowd-sourced' vs 'retail') is stored as a metafield. Obviously these values change over time. It may be useful for departments within your business (like 3PL and finance) to have a snapshot of the phase at the time of purchase. To do that we suggest adding a hidden field on your Product page using logic like this:

``` twig 
{% assign currentPhase = 'retail' %}
{% include 'tsio-workshop-data-product', wsProduct: product %}
{%- if wsData -%}
  {% assign currentPhase = wsPhase %}
{% endif %}
<input type="hidden" name="properties[_Phase]" value="{{ currentPhase }}">
```

In the above we pre-assign the value as ‘retail’ and then re-assign it if Workshop data is present. The hidden field will then pass the phase through the cart and checkout as a [Line Item Property.](https://help.shopify.com/en/themes/customization/products/features/get-customization-information-for-products) (In the above the property is made invisible to the customer in checkout by the leading underscore, you can simply remove that to make it visible.) __Note:__ that this hidden field may or may not be enough to work with an AJAX add-to-cart function, depending on the particulars of your theme.

### 4. Custom Collections Install

The app can maintain three Custom Collections within your store:

1. __Workshop Projects: Active__ can be accessed by handle `workshop-projects-active`
2. __Workshop Projects: Active Pre-Orders__  can be accessed by handle `workshop-projects-active-pre-orders`
3. __Workshop Projects: In-Production__ can be accessed by handle `workshop-projects-in-production`

### 5. Landing Page Install

__TBC__

<hr>

__Pending Updates:__

* [x] Conversion of Project end date to animated countdown.
* [x] Inclusion of estimated delivery date(s) within admin
* [x] Inclusion of estimated delivery date(s) on front-end
* [ ] Inclusion of order cut off date(s) when multiple ship dates, within admin
* [x] Inclusion of order cut off date(s) when multiple ship dates, on front-end
* [x] Regular Collection template snippet includes.
* [ ] Landing Page template.
