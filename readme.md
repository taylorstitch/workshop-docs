# The Workshop

Crowd-Funding app for Shopify stores.  (beta)

## Install Procedure

### 1. Store Authentication

We are not yet public and so not listed in the Shopify App Store officially, although there is a submission in progress&hellip; For now stores can install via the following URL, substituting your permanent `myshopify` subdomain in for `{{ store }}` and copying into your favorite web browser:

```
https://tsio-workshop.herokuapp.com/?shop={{ store }}.myshopify.com
```

### 2. Webhook Install

The application receives `order.paid` and `order.refund` webhooks from your store so that it can track sales over time and progress towards your goals, in realtime. You can install these webhooks via the "Settings" tab within the app admin.

### 3. App Assets Install

The application will add various code snippets to your store's primary theme, for inclusion on Product & Collection templates. **It will not publish these to your theme's front-end**, it simply adds the code snippets and makes them available to the the theme files so that you can use the following Liquid to add related elements in a places you deem appropriate:

#### 3.1 Product Section

```
{% section 'tsio-workshop-product' %}
```

The purpose of this snippet is to display goal progress, a remaining time countdown [and estimated ship dates tbc].

##### Product Section Styling

Various aspect of this section — including the color(s), size and style of the progress bar can be altered via your theme setting panel, when viewing a Product page that has the included workshop section with an active campaign. The rendered HTML structure of the included elements is also shown below, for the purposes of further customization styling beyond what is inherited from your theme (typography, font sizing, etc.):

``` html
<section class="ws-data">
  <ul>
    <li class="ws-period">
      <span>Workshop ends:</span>
      <time>02/03/2019</time>
    </li>
    <li class="ws-progress">67% Funded</li>
  </ul>
</section>
```

Some basic layout styles are applied but easily overridden by targeting the element classes specified above.

#### 3.2 Other Theme Elements

##### Product Meta Data

The following snippet, when included on a Product page or within a Product loop, will set Liquid variables for rendering or performing logic on:

```
{% include 'tsio-workshop-data-product', wsProduct: product %}
```

Variables set up:

* `wsPhase` string where possible values are `crowd-sourced`, `pre-order`, `in-production`, `retail`
* `wsActive` boolean true when Product phase is `crowd-sourced` or `pre-order`
* `wsStarts` date of Workshop campaign start
* `wsEnds` date of Workshop campaign end
* `wsTarget` number representing sales target  
* `wsSales` number representing current sales
* `wsProgress` percentage (without `%`) of wsSales/wsTarget
* `wsShipDates` array of (max 3) potential ship dates

### 4. Custom Collections Install

The app can maintain three Custom Collections within your store:

1. __Workshop Projects: Active__ can be accessed by handle `workshop-projects-active`
2. __Workshop Projects: Active Pre-Orders__  can be accessed by handle `workshop-projects-active-pre-orders`
3. __Workshop Projects: In-Production__ can be accessed by handle `workshop-projects-in-production`

### 5. Landing Page Install

__TBC__

<hr>

__Pending Updates:__

* [x] Conversion of campaign end date to animated countdown.
* [x] Inclusion of estimated delivery date(s) within admin
* [ ] Inclusion of estimated delivery date(s) on front-end
* [ ] Inclusion of order cut off date(s) when multiple ship dates, within admin
* [ ] Inclusion of order cut off date(s) when multiple ship dates, on front-end
* [ ] Regular Collection template snippet includes.
* [ ] Landing Page template.