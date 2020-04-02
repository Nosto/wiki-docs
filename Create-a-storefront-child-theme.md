Do not directly modify Magento's built in themes, instead create a new custom theme.

The minimal steps required to add a new theme in the Magento system are the following:

- Create a directory for the theme under
 ```
 app/design/frontend/<your_vendor_name>/<your_theme_name>
 ```
- Add a declaration file `theme.xml` 
	``` xml
    <theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
         <title>You theme name</title> <!-- your theme's name -->
         <parent>Magento/blank</parent> <!-- the parent theme, in case your theme inherits from an existing theme -->
    </theme>```
- Add `registration.php`.
     ``` php
     <?php
         \Magento\Framework\Component\ComponentRegistrar::register(
              \Magento\Framework\Component\ComponentRegistrar::THEME,
              'frontend/<vendor>/<theme_name>',
              __DIR__
         );
     ```

- Activate your theme in the Admin panel.
     - Go to Content on the sidebar

     ![menubar_design](https://user-images.githubusercontent.com/2778820/36483253-4c8532d2-171e-11e8-95b2-ba129bd0ca2c.png)

     - Select configuration under Design
     - Click on your recently created theme
     - Select the Edit action to your corresponding to your store view
     - On the Default Theme section, select your recently created theme in the dropdown

     ![storeview_theme](https://user-images.githubusercontent.com/2778820/36483340-826a6c5a-171e-11e8-93de-449f5744d7ec.png)

     - Save the configuration
     - Clear Magento's cache

