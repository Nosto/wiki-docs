## Overriding block templates

The block templates includes all the meta-data ("tagging") that is placed in your store, and sent to Nosto while your users are browsing the pages. The tagging is populated automatically, but if you wish to do some customisation it is easy to override these templates.

This is done by simply copying the template file into your theme directory and editing it. Magento will know about the overridden file automatically after the cache is cleared.

Please note that the product tagging should be overridden as describe in the previous section. This is important because product data is also transferred to Nosto using an API and not only through the tagging. If you only override the block template, then Nosto will receive mixed information about the same products.

Example:

Overriding the logged in customer tagging.

Copy `/app/design/frontend/base/default/template/nostotagging/customer.phtml` to `/app/design/frontend/THEME/default/template/nostotagging/customer.phtml` and then edit the file.

### Moving recommendations

To move the default recommendation blocks on your site, you can define the new positions in your theme `local.xml` file located in your theme folder under `app/design/frontend/THEME/default/layout/local.xml`. If the file does not exists, just create it. Then define the rules inside the `<layout>` section.

Example:

Moving the topmost recommendation block on the category page to be positioned after the bottommost one.

```xml
    <catalog_category_default>
        <update handle="nosto_move_category_recommendations" />
    </catalog_category_default>
    <catalog_category_layered>
        <update handle="nosto_move_category_recommendations" />
    </catalog_category_layered>
    <nosto_move_category_recommendations>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.category1</name></action>
            <action method="insert">
                <blockName>nosto.page.category1</blockName>
                <siblingName>nosto.page.category2</siblingName>
                <after>1</after>
            </action>
        </reference>
    </nosto_move_category_recommendations>
```

### Removing recommendations

To remove the default recommendation blocks from being rendered by the extension, you can remove them from the layout. This is useful in case you wish to include the recommendations yourself in your theme, but want to keep the rest of the extension functionality.

To remove them from the layout, open the `local.xml` file located in your theme folder under `app/design/frontend/THEME/default/layout/local.xml`. If the file does not exists, just create it. Then add the following lines inside the `<layout>` section of the file.

```xml
    <default>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.top</name></action>
            <action method="unsetChild"><name>nosto.column.left</name></action>
            <action method="unsetChild"><name>nosto.column.right</name></action>
            <action method="unsetChild"><name>nosto.page.footer</name></action>
        </reference>
    </default>
    <cms_index_index>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.home1</name></action>
            <action method="unsetChild"><name>nosto.page.home2</name></action>
            <action method="unsetChild"><name>nosto.page.home3</name></action>
            <action method="unsetChild"><name>nosto.page.home4</name></action>
        </reference>
    </cms_index_index>
    <checkout_cart_index>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.cart1</name></action>
            <action method="unsetChild"><name>nosto.page.cart2</name></action>
            <action method="unsetChild"><name>nosto.page.cart3</name></action>
        </reference>
    </checkout_cart_index>
    <catalog_product_view>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.product1</name></action>
            <action method="unsetChild"><name>nosto.page.product2</name></action>
            <action method="unsetChild"><name>nosto.page.product3</name></action>
        </reference>
    </catalog_product_view>
    <catalogsearch_result_index>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.search1</name></action>
            <action method="unsetChild"><name>nosto.page.search2</name></action>
        </reference>
    </catalogsearch_result_index>
    <catalog_category_default>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.category1</name></action>
            <action method="unsetChild"><name>nosto.page.category2</name></action>
        </reference>
    </catalog_category_default>
    <catalog_category_layered>
        <reference name="content">
            <action method="unsetChild"><name>nosto.page.category1</name></action>
            <action method="unsetChild"><name>nosto.page.category2</name></action>
        </reference>
    </catalog_category_layered>
```

Note that you can also just remove the blocks you want to, instead of removing all of them.