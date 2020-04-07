# FAQ

## How do I view module errors?

When the module encounters an error, it logs it in the PrestaShop error log. You can view this log from inside the PrestaShop backend, under `Advanced Parameters -> Logs`. When encountering an error with the module, please attach the log when reporting the issue to Nosto.

## How can I clear my template cache?

1. Log into Prestashop admin panel.
2. Navigate to **Advanced Parameters** &gt; **Performance**.
3. Set **Force compile** to **Yes**, disable **Cache** and save changes.

## Why are my product image URLs incorrect, broken or showing a different image?

Each product in Prestashop can have multiple images. Only one of these images as defined as a "cover" image in Prestashop. The cover image is the default image of the product.

In Prestashop, there are multiple image versions defined. Each image-version has a given width and height. You can view a list of all the image-versions by navigating to "Preferences" / "Images".

We try to fetch the image-version that is closest to a width of 450px and is enabled for "Products". We then try to get the cover image in that matching image-version.

In the event that the product tagging has broken or invalid image URLs, you will need to regenerate all the thumbnails by navigating to "Preferences" / "Images" and configuring the last panel named "Regenerate Thumbnails". In the "Select an image" dropdown choose "Products", and in the "Erase previous images" switch below, choose "No". Then click the "Regenerate Thumbnails" button to begin the process.

This will cause Prestashop to regenerate all the product thumbnails. Please exercise caution when clicking this button as this is a slow operation. Please note that in order regenerate thumbnails, you will need to have write permissions on the `img` directory. You can view a list of all your configuration errors by navigating to "Configuration Information" / "Advanced Parameters" and viewing the "Check your Configuration" panel. Problems relating to missing write permissions on image and thumbnail directories are listed here. You will need to rectify these warnings before regenerating thumbnails.

## Why do I get SSL errors about self-signed certificates?

If you get an error that reads something along the lines of "SSL certificate problem: self signed certificate in certificate chain", then there is an error with your SSL certificates.

This error will affect all outbound API calls and will prevent you from creating and account, reconnecting an account or making use of any of the API.

Nosto does not allow self-signed certificates and you must use from a trusted provider. SSL certificates are free and you can get a free one from a provider such as [Let's Encrypt ](https://letsencrypt.org/)

Use a free service such as [Qualys SSL Labs](https://www.ssllabs.com/ssltest/) to verify your SSL configuration:

![arating](https://cloud.githubusercontent.com/assets/327432/25840303/f80b447e-34a3-11e7-9696-4146257d0eee.png)

## How to reset Nosto module

If the tagging or other Nosto features disappear for example after Prestashop update you need to reset Nosto module. Resetting a module can be done in your store admin. After logging in your store admin navigate to "Modules and service" and locate Nosto module. The location of Nosto module varies a bit depending on Prestashop version. Once you have located the module open the drop down in the module panel and choose "Reset" \(see image below\). Confirm the reset if prompted. After resetting re-connect Nosto account to your store. ![Reset Nosto module](https://user-images.githubusercontent.com/15191701/32780631-b4f75a7e-c94a-11e7-8de3-1761beb959f5.png)

