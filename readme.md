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

The application will add a code snippet to your store's primary theme, for inclusion on Product pages. The purpose of the snippet is to display goal progress and a remaining time countdown. **It will not publish this on your theme's front-end**, it simply adds the code snippet to the the theme files so that you can use the following Liquid to add those elements in a place you deem appropriate:

#### 3.1 Snippet Include

```
{% include 'tsio-workshop' %}
```

#### 3.2 Snippet Styling

The rendered HTML structure of the included elements, for the purposes of styling (colors, fonts, etc.) is as follows:

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

<hr>

__Pending Updates:__

* Conversion of campaign end date to animated countdown.
* Inclusion of estimated delivery date(s) within admin & front-end.
* Regular Collection template snippet includes.
* Combined Collection template.