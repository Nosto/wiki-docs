Adding Nosto email widgets to the emails sent from Magento 2 is a straight forward process. First you need create email campaign(s) in [Nosto admin](https://my.nosto.com) and simply copy paste the HTML into your email template.

## Creating email widget campaigns

In order to use email widgets in Magento 2 email templates you must first create widgets (or campaigns) in Nosto. [This help article](https://help.nosto.com/email-widget/getting-started/getting-started-with-email-personalization-widgets) will guide you through the process of creating email campaigns.

You will only need to create email campaigns once.

## Modifying the email templates in Magento 2

After you have created the email campaigns in Nosto you can add the widgets into your Magento 2 email templates.

**Please note that the vistor's email variable may vary depeneding on the template type / template context.**

### 1. Open email template editor
Navigate to "Marketing > Email templates"
![m2-email-widget-1](https://user-images.githubusercontent.com/15191701/42504563-e12ad156-8444-11e8-9f09-37aef9becbd8.png)

### 2. Modify or add a new template
If you wish to add email widgets into an existing template choose the one you wish to add widgets to or create a new template by clicking "Add New Template".
![email_templates___communications___marketing___magento_admin](https://user-images.githubusercontent.com/15191701/42505047-443e1270-8446-11e8-8d33-5a336e0d7c6f.png)

### 3. Load existing template
When creating a new template it might be a good idea to use existing one as a starting point. You can load existing template by choosing a template from the drop down menu and by clicking "Load Template".
![m2-email-widget-2](https://user-images.githubusercontent.com/15191701/42504564-e1473940-8444-11e8-8ed6-8d70f191673c.png)

### 4. Copy and paste the HTML from Nosto email widgets
Last thing to do is to copy the HTML snippet from Nosto and paste it to the email template. First double check that the email variable is available in Magento 2 template context and click "Copy to Clipboard".
![email_widgets](https://user-images.githubusercontent.com/15191701/42506615-13d97c50-844b-11e8-86cf-42afed11e27b.png)

Finally paste the snippet into your template to wherever you want the recommendations to appear. You can add multiple email widgets into a template. 

![m2-email-widget-3](https://user-images.githubusercontent.com/15191701/42504565-e162ea6e-8444-11e8-86b7-b997741deaa6.png)

