# The Workshop App

Crowd-Funding app for Shopify stores. \(beta\)

## 1. Shopify Store Authentication

We are not yet public and so not listed in the Shopify App Store officially, although there is a submission in progress… For now stores can install via the following URL, substituting your permanent `myshopify` subdomain in for `{{ store }}` and copying into your favorite web browser:

```markup
https://tsio-workshop.herokuapp.com/?shop={{ store }}.myshopify.com
```

### Webhook Install

The application installs and receives `order.paid` and `order.refund` webhooks from your store so that it can track sales over time and progress towards your goals, in realtime.

### App Assets Install

The application will add various code snippets to your store's primary theme, for inclusion on Product & Collection templates. See [Theme](../code-repository/theme/).

{% hint style="info" %}
  **It will not publish these to your theme's front-end**, it simply adds the code snippets and makes them available to the the admin and theme files so that you can use the Liquid documented here to add related elements in any places you deem appropriate.
{% endhint %}

### Custom Collections Install

The app maintains three Custom Collections within your store:

1. **Workshop Projects: Active** can be accessed by handle `workshop-projects-active`
2. **Workshop Projects: Active Pre-Orders**  can be accessed by handle `workshop-projects-active-pre-orders`
3. **Workshop Projects: In-Production** can be accessed by handle `workshop-projects-in-production`

### Landing Page Install

If the Custom Collections mentioned above have not been assigned the `collection.tsio-workshop` template then you should do that :)  
