# Customizing The Order Confirmation Email

The purpose of the Liquid below is to help ID different phases of Products within the line\_items loop, so that they can be treated differently. \(Note that we also separate out Gift Cards. This may or may not be applicable / useful to your shop.\)

```liquid
{% raw %}
{% comment %}
  Identify and items that are crowd-funding workshop, pre-order, and items that are in-stock (inc. Gift Cards separately)
  i.e. line.properties['_Phase'] = 'crowd-sourced' or 'retail'
{% endcomment %}

{% assign hasNoPhaseProducts = false %}
{% assign hasWorkshopProducts = false %}
{% assign hasPreOrderProducts = false %}
{% assign hasRetailProducts = false %}
{% assign hasGiftCardProducts = false %}

{% for line in line_items %}

  {% assign product = line.product %}

  {% assign isNoPhaseProduct = false %}
  {% assign isWorkshopProduct = false %}
  {% assign isPreOrderProduct = false %}
  {% assign isRetailProduct = false %}
  {% assign isGiftCardProduct = false %}

  {% if product.type == 'Gift Certificate' %}
    {% assign isGiftCardProduct = true %}
    {% assign hasGiftCardProducts = true %}
  {% else %}

    {% assign LIPs = line.properties %}
    {% assign LIPCount = LIPs | size %}
    {% if LIPCount > 0 %}
      {% case LIPs['_Phase'] %}
        {% when 'crowd-sourced' %}
          {% assign isWorkshopProduct = true %}
          {% assign hasWorkshopProducts = true %}
        {% when 'pre-order' %}
          {% assign isPreOrderProduct = true %}
          {% assign hasPreOrderProducts = true %}
        {% when 'retail' %}
          {% assign isRetailProduct = true %}
          {% assign hasRetailProducts = true %}
        {% else %}
          {% assign isNoPhaseProduct = true %}
          {% assign hasNoPhaseProducts = true %}
      {% endcase %}
    {% else %}
      {% assign isNoPhaseProduct = true %}
      {% assign hasNoPhaseProducts = true %}
    {% endif %}

  {% endif %}

  {% comment %}
    Uncomment the following to "debug" inline:

    <p>
      <span class="bold">Debug:</span><br />
      isGiftCardProduct: {{{{raw}}}}{{ isGiftCardProduct }}{{{{/raw}}}}<br />
      isNoPhaseProduct: {{{{raw}}}}{{ isNoPhaseProduct }}{{{{/raw}}}}<br />
      isWorkshopProduct: {{{{raw}}}}{{ isWorkshopProduct }}{{{{/raw}}}}<br />
      isPreOrderProduct: {{{{raw}}}}{{ isPreOrderProduct }}{{{{/raw}}}}<br />
      isRetailProduct: {{{{raw}}}}{{ isRetailProduct }}{{{{/raw}}}}
    </p>
  {% endcomment %}


  {% comment %}
    Here you can do some logic on the above variables, either printing out the line items with a different alert or capturing them to put them in a different section
  {% endcomment %}

{% endfor %}

{% comment %}
  Uncomment the following to "debug" inline:

  <p>
    <span class="bold">Debug:</span><br />
    hasNoPhaseProducts: {{{{raw}}}}{{ hasNoPhaseProducts }}{{{{/raw}}}}<br />
    hasWorkshopProducts: {{{{raw}}}}{{ hasWorkshopProducts }}{{{{/raw}}}}<br />
    hasPreOrderProducts: {{{{raw}}}}{{ hasPreOrderProducts }}{{{{/raw}}}}<br />
    hasRetailProducts: {{{{raw}}}}{{ hasRetailProducts }}{{{{/raw}}}}<br />
    hasGiftCardProducts: {{{{raw}}}}{{ hasGiftCardProducts }}{{{{/raw}}}}
  </p>
{% endcomment %}
{% endraw %}
```

