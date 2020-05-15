# Implement on your website

### Choosing the implementation method

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Implementing by a Nosto Extension</td>
      <td style="text-align:left">
        <p><a href="https://docs.nosto.com/magento/">Magento</a>
        </p>
        <p><a href="https://docs.nosto.com/magento-2/">Magento 2</a>
        </p>
        <p><a href="https://docs.nosto.com/shopware">Shopware</a>
        </p>
        <p><a href="https://docs.nosto.com/shopify">Shopify</a>
        </p>
        <p><a href="https://docs.nosto.com/prestashop">Prestashop</a>
        </p>
        <p><a href="https://docs.nosto.com/bigcommerce">BigCommerce</a>
        </p>
        <p><a href="https://docs.nosto.com/salesforce">Salesforce</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Implement on your SPA</td>
      <td style="text-align:left"><a href="../../apis/frontend/implementation-guide-session-api/">Session API</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Implement on your PWA</td>
      <td style="text-align:left"><a href="../../apis/frontend/implementation-guide-session-api/">Session API</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Implement on your Headless</td>
      <td style="text-align:left"><a href="../../apis/graphql-an-introduction/">GraphQL</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Implement on a standard e-commerce store</td>
      <td style="text-align:left"><a href="manual-implementation/">Manual Tagging</a>
      </td>
    </tr>
  </tbody>
</table>### Product Updates in case of SPA and PWA environments

When implementing in SPA and PWA environments, product updates must be done via [REST API](../../apis/rest/). In case you are using some of the platforms that Nosto has extension for the extension takes care of the product updates.  

### SPA / PWA on top of a platform that Nosto has extension for  

If you have implemented SPA / PWA on top a platform that Nosto has extension for you will still need to implement the frontend part using Session API. The extension will take care of the product updates, order confirmations, exchange rates and other background processes but displaying the recos, popups, etc. must be done using Session API.   

### When dynamic functionality is needed / no page reload

In case your website implement some dynamic functionality, you can use the [JS API](../../apis/js-apis/).  
_Note that you can not mix Session API and JS API._

### 

|  |
| :--- |




