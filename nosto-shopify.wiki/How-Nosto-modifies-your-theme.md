Upon installing the Nosto for Shopify app, choosing a theme to edit, Nosto amends the core theme file called `layout/theme.liquid` and amends it to include the Nosto semantic markup snippet and the Nosto script snippet. To better understand snippets and the Shopify theme structure, we recommend reading [Shopify's theme development guide](https://help.shopify.com/themes/development/templates#snippets).

## Amendments to include the Nosto script

Nosto amends the core theme file (`layout/theme.liquid`) to include the Nosto semantic markup snippet using the following include:

```liquid
{% include 'nosto-script' with 'shopify-<id>' %}
```

> **Note:** The `<id>` variable in the above script is a unique identifier for your store. When working with multiple top-level domains e.g. shop.co.uk, shop.com, extreme caution must be exercised to ensure that you do not mix up the unique references across stores. This will cause the recommendations to be polluted with customer data leaking across store views. In the event that you need guidance with handling multiple accounts, please contact our support personnel.

A corresponding snippet file named `nosto-script.liquid` is uploaded to the `snippets` directory of your theme. The snippet contains the one-liner `script` element to load the Nosto script.

```liquid
<script type="text/javascript" src="//connect.nosto.com/include/{{ nosto-script }}" async></script>
```

## Amendments to include the Nosto tagging

Nosto also amends the core theme file (`layout/theme.liquid`) to include the Nosto semantic markup snippet using the following include:

```liquid
{% include 'nosto-tagging' %}
```

A corresponding snippet file named `nosto-tagging.liquid` is uploaded to the `snippets` directory of your theme. The snippet contains all the Liquid templates needed to render markup that gives Nosto insights into the page context

```liquid
{% if template == 'index'%}
<div class="nosto_page_type" style="display:none">front</div>
{% elsif template == '404'%}
<div class="nosto_page_type" style="display:none">notfound</div>
{% elsif template == 'cart'%}
<div class="nosto_page_type" style="display:none">cart</div>
{% endif %}

{% if product %}
<div class="nosto_page_type" style="display:none">product</div>
<div class="nosto_product" style="display:none">
    <span class="url">{{shop.url}}{{product.url}}</span>
    <span class="product_id">{{product.id}}</span>
    <span class="name">{{product.title}}</span>
    <span class="price">{{product.variants[0].price | money_without_currency | remove: ','}}</span>
    <span class="image_url">{{product.featured_image | img_url: 'master' }}</span>
    <span class="thumb_url">{{product.featured_image | img_url: 'large' }}</span>
    <span class="price_currency_code">{{shop.currency}}</span>
    {% if product.available %}
    <span class="availability">InStock</span>
    {% else %}
    <span class="availability">OutOfStock</span>
    {% endif %}
    <span class="category">{{product.type}}</span>
    {% for collection in product.collections %}
    <span class="category">{{collection.title}}</span>
    {% endfor %}
    <span class="description">{{product.description}}</span>
    {% if product.variants[0].compare_at_price > product.variants[0].price%}
    <span class="list_price">{{product.variants[0].compare_at_price | money_without_currency | remove: ','}}</span>
    {% endif %}
    <span class="brand">{{product.vendor}}</span>
    {% for tag in product.tags %}
    <span class="tag1">{{ tag }}</span>
    {% endfor %}
    {% if product.variants.size == 1 %}
    <span class="tag2">add-to-cart</span>
    {% endif %}
    <span class="tag3"></span>
    <span class="date_published">{{product.published_at | date:'%Y-%m-%d'}}</span>
    {% for image in product.images %}
    {% if image != product.featured_image %}
    <span class="alternate_image_url">{{ image.src | img_url: 'master' }}</span>
    {% endif %}
    {% endfor %}
    {% for variant in product.variants %}
    <span class="nosto_sku">
      <span class="id">{{ variant.id }}</span>
      <span class="name">{{ variant.title  }}</span>
      <span class="price">{{ variant.price | money_without_currency | remove: ',' }}</span>
      <span class="list_price">{{ variant.compare_at_price | money_without_currency | remove: ',' }}</span>
      <span class="image_url">{{ variant.image.src | img_url: 'master' }}</span>
      <span class="url">{{ shop.url }}{{ variant.url }}</span>
      {% if variant.available %}
      <span class="availability">InStock</span>
      {% else %}
      <span class="availability">OutOfStock</span>
      {% endif %}
      <span class="custom_fields">
        {% for option in product.options %}
        {% case forloop.index %}
        {% when 1 %}
        <span class="{{ option }}">{{ variant.option1 }}</span>
        {% when 2 %}
        <span class="{{ option }}">{{ variant.option2 }}</span>
        {% when 3 %}
        <span class="{{ option }}">{{ variant.option3 }}</span>
        {% endcase %}
        {% endfor %}
      </span>
    </span>
    {% endfor %}
</div>
{% endif %}

{% if collection %}
{% unless product %}
<div class="nosto_page_type" style="display:none">category</div>
<div class="nosto_category" style="display:none">{{ collection.title }}</div>
{% for tag in current_tags %}
<span class="nosto_tag" style="display:none">{{ tag }}</span>
{% endfor %}
{% endunless %}
{% endif %}

{% if search.performed %}
<div class="nosto_page_type" style="display:none">search</div>
<div class="nosto_search_term" style="display:none">{{ search.terms }}</div>
{% endif %}

{% if cart %}
<div class="nosto_external_visit_ref" style="display:none"></div>
<script type="text/javascript">
  var ctoken = (document.cookie.match('(^|; )cart=([^;]*)')||0)[2];
  if (ctoken) {
    document.getElementsByClassName('nosto_external_visit_ref')[0].textContent = ctoken;
  }

</script>
<div class="nosto_cart" style="display:none">
    {% assign restorecart = '' %}
    {% for line_item in cart.items %}
    <div class="line_item">
        {% if restorecart != '' %}
        {% assign restorecart = restorecart | append: ',' %}
        {% endif %}
        {% assign restorecart = restorecart | append:  line_item.variant_id | append: ':' |append: line_item.quantity %}
        <span class="product_id">{{ line_item.product.id }}</span>
        <span class="sku_id">{{ line_item.variant_id }}</span>
        <span class="quantity">{{ line_item.quantity }}</span>
        <span class="name">{{ line_item.title }}</span>
        <span class="unit_price">{{ line_item.price | money_without_currency | remove: ',' }}</span>
        <span class="price_currency_code">{{ shop.currency }}</span>
    </div>
    {% endfor %}
    {% if restorecart != '' %}
    <span class="restore_link">{{ shop.secure_url }}/cart/{{restorecart}}</span>
    {% endif %}
</div>
{% endif %}

{% if customer %}
<div class="nosto_customer" style="display:none">
    <span class="email">{{ customer.email }}</span>
    <span class="first_name">{{ customer.first_name }}</span>
    <span class="last_name">{{ customer.last_name }}</span>
    <span class="customer_reference">{{ customer.id | append: shop.permanent_domain | sha256 }}</span>
    {% if customer.accepts_marketing %}
    <span class="marketing_permission">true</span>
    {% else %}
    <span class="marketing_permission">false</span>
    {% endif %}
</div>
{% endif %}

{% if checkout %}
<div class="nosto_page_type" style="display:none">cart</div>
<div class="nosto_external_visit_ref" style="display:none"></div>
<script type="text/javascript">
  var ctoken = (document.cookie.match('(^|; )cart=([^;]*)')||0)[2];
  if (ctoken) {
    document.getElementsByClassName('nosto_external_visit_ref')[0].textContent = ctoken;
  }
</script>
<div class="nosto_cart" style="display:none">
    {% for line_item in checkout.line_items %}
    <div class="line_item">
        <span class="product_id">{{ line_item.product.id }}</span>
        <span class="quantity">{{ line_item.quantity }}</span>
        <span class="name">{{ line_item.title }}</span>
        <span class="unit_price">{{ line_item.price | money_without_currency | remove: ',' }}</span>
        <span class="price_currency_code">{{ shop.currency }}</span>
    </div>
    {% endfor %}
</div>
{% endif %}
```