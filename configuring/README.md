# Configuring

## Custom Preferences

Custom Preferences \(Merchant Tools &gt; Site Preferences &gt; Custom Preferences\) are used for configuring Nosto accounts and product attribute mapping.

![sandbox\_ \_refarch\_ \_business\_manager](https://user-images.githubusercontent.com/15191701/53867965-99bbac80-3ffd-11e9-80d8-a8b878eeaab0.png)

From the configuration page you can enable / disable Nosto, map locales to Nosto accounts and define the product attribute mapping.

![sandbox\_ \_nosto\_settings\_ \_business\_manager](https://user-images.githubusercontent.com/15191701/53871401-dd65e480-4004-11e9-8dd5-1a0b69192b46.png)

### Enable Nosto tagging

This site preference is to enable/ disable Nosto functionality at a site level

### Nosto Script link

Here you will configure Nosto account \(or Nosto script source\) for each locale. Please note that each site and each locale inside a site must have separate Nosto account.

You will map the locale to a Nosto script source in JSON format. Locale is the key and the script source. If you would for example have two locales \(en\_US and en\_GB\) you would need two Nosto account \(example-nosto-1, example-nosto-2\). The JSON would then be

```javascript
{
  "en_US": "<script src='//connect.nosto.com/include/example-nosto-1' async></script>",
  "en_GB": "<script src='//connect.nosto.com/include/example-nosto-2' async></script>"
}
```

### Nosto Product attribute configuration

This preference contains product attributes and mapping to Nosto product tagging. In practice the key is the field in Nosto product tagging and the value is corresponding field in Salesforce Commerce Cloud product object.

If the key is not a standard Nosto product tag the key and the value will be pushed into that custom fields. In the example mapping below `size` and the value of the size will be pushed into the custom fields.

For configurable products the same attribute mapping is applied for "main product" and variations.

```javascript
{
  "description": "longDescription",
  "brand": "brand",
  "rating_value": "custom.ratingValue",
  "size": "custom.size"
}
```

Please not that if the product tagging is not appearing on product pages it is most likely due to invalid product attribute mapping. Most common case is that some Nosto product attribute is mapped to a non-existent SFCC product attribute. If a product attribute doesn't have value the it is omitted from the Nosto product tagging.

## Nosto Brand Configuration \(Category Level\)

This setting is used for configuring the brand for Nosto product tagging. In other words, which category should be used a product brand in Nosto recommendations.

Navigate to the targeted catalog \(Merchant Tools &gt; Products and Catalogs &gt; Catalogs\) and click on it to open it in edit mode. You can see setting "Is Nosto Brand" under "Category Attributes" tab. You can find Nosto category settings under Category Attributes tab.

![sandbox\_ \_refarch\_ \_business\_manager](https://user-images.githubusercontent.com/15191701/53877107-f4aacf00-4010-11e9-96ac-da5a918c4071.png)

