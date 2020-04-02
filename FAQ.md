## How does the tax calculation work ![2.3.2](https://img.shields.io/badge/nosto-2.3.2-green.svg)

Nosto follows the same logic for product prices than Shopware. If you configure a store's customer group to be displayed products including taxes ("Gross price displayed in frontend:") Nosto products will be also tagged including taxes. If you leave this checkbox (see screenshot below) empty the prices will displayed without taxes and Nosto product prices will be tagged without taxes.   
    
![shopware_5_4_0_dev__rev__290328060__-_backend__c__shopware_ag](https://user-images.githubusercontent.com/15191701/51747544-82aaa600-20b2-11e9-92ba-72b68aa3532a.png)

Nosto follows the same tax calculation logic than Shopware. In short, if a tax class contains only the default tax rate that will be the rate that is applied to all products using this tax class. If any additional tax rules are configured the default tax rate is omitted and only the additional tax rules are applied. 

![shopware_5_4_0_dev__rev__290328060__-_backend__c__shopware_ag](https://user-images.githubusercontent.com/15191701/51747468-46774580-20b2-11e9-8201-e0ec6a3d3919.png)

**Please note that Nosto does not take into account the country when applying tax rules. If a matching customer group is found Nosto will apply the tax rule regardless of the country**

## How does Nosto pickup the product data for configurable products
As Nosto does not index variations separately, it uses one variation as a "main product". Nosto picks up the values from the preselected (see screenshot below) variation for the main product. In other words Nosto picks the product price and image url from the preselected variation.  

![shopware-preselection](https://user-images.githubusercontent.com/15191701/40541338-07acc27a-6024-11e8-8c0a-e0c7c809eecd.png)

## How do I view plugin errors?

When the plugin encounters an error, it logs it in the Shopware plugins error log folder. You can find the log file in `SHOPWARE/var/logs/plugin_production-YYYY-MM-DD.log` on your server. When encountering an error with the plugin, please attach the log when reporting the issue to Nosto.