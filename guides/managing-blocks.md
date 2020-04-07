# Managing Blocks

### First steps

Set application to development mode with

```bash
bin/magento deploy:mode:set developer
```

The preferred method to edit Nosto's block data is by [Extending Nosto's Module](overriding-or-extending-functionalities/).  
 However you can also [Create a Storefront Child Theme](https://github.com/supercid/wiki-docs/tree/bfcad31e54e9c48d9a2ca1df7b28ae4691497212/Create-a-storefront-child-theme.md). Notice that by creating a new child theme, all widgets defined in the parent theme **will not be inherit** by the child theme, you would have to manually recreate all of them.   
  


## Managing Blocks

### Enable Template Path Hints for Storefront

In order to identify which block, or which template is being used in Magento's front end blocks, we need to activate `Template Path Hints`

* On the Magento 2 Admin Panel, click on `Stores`, then `Configuration` under the Settings tab
* Scroll down and uncollapse the `Advanced` group, now select `Developer`
* Under the `Debug` group you will find `Enabled Template Path Hints for Storefront`, select `Yes` from the dropdown menu and hit the `Save Config` button.

Your frontend now should look like this: ![screen shot 2018-02-26 at 11 52 47](https://user-images.githubusercontent.com/2778820/36664059-349c6272-1aec-11e8-9a8e-ecebd0a616d2.png)

## Add Layout and Template Files - Overriding

After identifying which template we want to override, in this example we will use the `element.phtml` template, we will now look at it's layout file located in the `Vendor\Nosto` folder of your Magento 2 Installation.

Navigate to `vendor/nosto/module-nostotagging/view/frontend/templates` and you will find the `element.phtml` Now, if we want to override this template, we need to create some directories according to the name of the theme or module we are overriding.

### Using a child theme

If you are using a child theme, you need to create a folder named `Nosto_Tagging`, which is the name of our module.  
 In the path `app/design/frontend/<vendor>/<theme_name>/Nosto_Tagging` we also need to create 2 new directories.

* Create a new folder named `layout` and another folder named `templates`
* Inside the `templates` directory, we put the new templates structure we want Nosto to use, so create the `element.phtml` file there with the following content:

```php
<div class="nosto_element" id="<?php echo $this->escapeHtml($this->getElementId()); ?>">
    <?php echo __('This is a superb custom text'); ?>
</div>
```

* Inside the `layout` folder we need to create a new file named default.xml, the basic structure for this file is:

  \`\`\`xml

  &lt;?xml version="1.0"?&gt;

```text
### Using the module extending solution
In case you have extended the Nosto module, you need to mirror the `vendor` folder structure.<br>
Create the following directory structure:
```bash
app
├── code
│   └── My
│       └── Nosto
│           └── view
│               └── frontend
│                   ├── layout
│                   └── templates
```

* Inside the `templates` directory, we put the new templates structure we want Nosto to use, so create the `element.phtml` file there with the following content:

```php
<div class="nosto_element" id="<?php echo $this->escapeHtml($this->getElementId()); ?>">
    <?php echo __('This is a superb custom text'); ?>
</div>
```

* Inside the `layout` folder we need to create a new file named default.xml, the basic structure for this file is:

  \`\`\`xml

  &lt;?xml version="1.0"?&gt;

```text
At the end, you should at least have the structure below:
```bash
app
├── code
│   └── My
│       └── Nosto
│           ├── composer.json
│           ├── etc
│           │   └── module.xml
│           ├── registration.php
│           └── view
│               └── frontend
│                   ├── layout
│                   │   └── default.xml
│                   └── templates
│                       └── element.phtml
```

Clear cache and visit Magento's FrontEnd, and you should see the text in Nosto's element.

![screen shot 2018-02-26 at 14 31 16](https://user-images.githubusercontent.com/2778820/36670715-ca1c7868-1b01-11e8-9c69-e0bed2dfbe3a.png)

## Moving Blocks Position

In order to move a block inside a page, we need to set the tag `move` in our layout file.   


* If you are using the child theme method, navigate to `app/design/frontend/<vendor>/<theme_name>/Nosto_Tagging/layout` 
* If you are using the module extension method, navigate to `app/code/My/Nosto/view/frontend/layout`  

Using the corresponding xml file, define which block you will move to where. In this case, let's use the `default.xml` to move the `Recommendations For You` block:

```markup
<body>
     <move element="nosto.page.home1" destination="header.container" as="new_alias"/>
</body
```

You can also use the attributes `after="-"` and `before="-"` to define exactly after or before which element you will put the new one. The dash `-` means all elements.

* The tag above will move the `nosto.page.home1` block to the very top of the page.
* Set the destination container in the `destination` tag attribute
* Set the new alias in the `as` attribute
* Additionally, you can find the Nosto block names in the xml files under the `%magento_installation_path%/vendor/nosto/module-nostotagging/view/frontend/layout` directory

![screen shot 2018-02-26 at 17 19 06](https://user-images.githubusercontent.com/2778820/36678371-370b9352-1b19-11e8-9de3-6250ea6abf94.png)

