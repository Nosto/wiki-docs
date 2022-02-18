Nosto recently implemented support for **Customer Group Pricing** feature, described [here](https://support.bigcommerce.com/s/article/Customer-Groups?language=en_US#pricing). With customer groups, offering product discounts and promotions gets even more convenient and flexible. In following sections, we will discuss more about this feature and how it can be activated from Nosto admin page.

> **Note:** In this initial release, nosto only supports discount rules for merchant default currency (say, EUR). When a product has no  discount rules matching the merchant default currency, then no recommendations will be served.

## Customer groups
A customer group is a set of rules for configuring product promotions or discounts using different pricing strategies as below,
* Product Rule - This rule will define discount rules for each product
* Category Rule - This rule will define rules for product categories. When defined, all products belonging to this category will received the discount offered. This can be further refined to include **direct** sub-categories of the selected product
* Store Rule - There can be only one store discount rule for a customer group. This rule will define discount rules for all the store products
* Price Lists - Price Lists are special form of defining discount rules. A store can have many price lists. This is similar to `Product Rule` discount rule except that price list offers flexibility of defining rules for each and every product at once. This can be used when there's a requirement for defining rules for huge number of products. 

**Note:** A customer group can only have a `Price List` rule OR Product and/or Category and/or store rule. These customer groups will then be associated with the customers.

Following are the pricing strategies that can be associated with pricing rules defined above
* Fixed pricing - Value defined will be the exact selling price of the product
* Percent pricing - Value defined will be the percentage value of the product list price. For e.g. if this value is 1, that means the product is offered at a 1% discount from the list price
* Price pricing - Value defined will be directly discounted from the product list price. For e.g. if this value is 100, that means the product price will be 100 less than the list price

**Note:** A Price List rule can only be defined using `Percent` pricing strategy.

### Rules precedence
Rules are applied in following order
1. Product Rule, if available
2. Price List Rule, if available
3. Category Rule, if available
4. Store Rule, if available

## Nosto's implementation
Nosto maps each customer group id as a product variation ID
for e.g. if the customer_group_id is `1`, then variation ID will be `1`.
In addition to Variation ID, a product variation has the following component,
* price - The selling price of the product including the discount as per the discount rule.
* list price - The actual price of the product excluding the discount.
* availability - Current status of product availability. For e.g. `In Stock` or `Out of Stock`.
* currency code - currency code of the product pricing rule. A rule can be defined in one currency but still be applied on different currency code. Consider, the product currency is `eur`, the customer is ordering using `usd` and the customer belongs to a customer group, 
In the situation, promotion will be converted to `usd` but  customer will still be paying in `eur` during checkout.

The json format of a single product variation will look like,

```json
{
  variation_id: 'VAR_1',
  price: 360,
  list_price: 260, //This can be a discount rule with PRICE method with a value of 100
  availability: 'InStock',
  price_text: '260',
  price_currency_code: 'eur'
}
```

In addition to the variations fetched, Nosto adds the default variation ID (as configured in "Nosto Admin"), where both price and list_price will have the same value. This is because Nosto requires all products to be associated with the default variation ID.

An example of default variation data,

```json
{
  variation_id: 'DEF_1',
  price: 360,
  list_price: 360, //There is no discount rule here. Product price is used as it is
  availability: 'InStock',
  price_text: '360',
  price_currency_code: 'eur' //Merchant currency code
}
```

## Activating customer group pricing in Nosto

### Currency Settings
In order to make use of the `Product Variation` approach, it needs to be enabled from Nosto admin. Please follow the steps outlined below,

Navigate to `Settings > Currency Settings` in Nosto admin, disable `Exchange rates` toggle and configure `Variation ID` which is considered the default variation id. If we consider our example in the previous section and configure the currency settings, it will look as shown below,

![](https://user-images.githubusercontent.com/82023195/154737808-5a86254f-88bd-4ddb-9fb7-0321eebbc163.png)

### Product Reindexing
After completing the set up under `Currency Settings`, re-index the products by following the below steps:
1. Navigate to Catalog Explorer > Products page 
2. Click "Update Catalog"

![](https://user-images.githubusercontent.com/82023195/154738159-823129bd-2e07-4262-8817-a1300dfa4963.png)
   
Note: Re-indexing may take some time to complete, depending on the product count. 
