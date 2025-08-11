# Product Images

This article covers how Nosto provides product images in on-site recommendations and helps you how to change the image settings if these look unsharp or otherwise bad to you. On the contrary, if recommendation images load slowly the article covers also how to adjust settings to speed up image load times and other common errors with the images.

## Introduction

The implementation of Nosto on a web-site maps out an original product image used on a product detail page (PDP) to Nosto automatically. In the background, an automated service fetches the default product image and creates eight different image versions out of the image, which are stored at Nosto and made available in our Content Delivery Network (CDN).

Some image versions are processed slightly by resizing, cropping and zooming the image a bit. Applicable image versions in Nosto’s CDN are:

| Version | Dimensions                                  |
|---------|---------------------------------------------|
| 1       | 170x170 pixels                              |
| 2       | 100x100 pixels                              |
| 3       | 90x70 pixels                                |
| 4       | 50x50 pixels                                |
| 5       | 30x30 pixels                                |
| 6       | 100x140 pixels                              |
| 7       | 200x200 pixels (original aspect ratio)      |
| 8       | 400x400 pixels (original aspect ratio)      |
| 9       | 750x750 pixels (original aspect ratio)      |

These are the the available image sizes via Nosto's CDN. Alternatively you can use original image , but in this case an image is loaded from your servers possibly affecting site load times, entirely depending on how images are hosted on your servers. Use following variable to use the original image.

```
$!product.imageUrl
```

The Nosto CDN thumbnails are accessible via

```
$!product.thumb(size)
```

where `size` is a number between 1-7 basd on the version mapping above.

## Custom thumbnail sizes

For Shopify and Bigcommerce Nosto supports also direct access to Shopify and Bigcommerce CDN thumbnails.

These are accessed via the following pattern:

```
$!product.image.getThumb(300, 300)
```

To access the first alternate image scaled down to a 300x300 image use

```
$!product.alternateImages[0].getThumb(300, 300)
```

The custom thumbnails are served without any cropping.

It is also possible to leave width or height undefined to define only a limit for one of the dimensions:

```
$!product.image.getThumb(300, null)
```

This would scale down the image to have a max width of 300 pixels.

## Troubleshooting Common Issues

### Product Images Don’t Appear

If a product image doesn’t appear or if the image link is broken this usually due to error in image url tagging, erroneous url mapping or a temporarily error while fetching the images from your site to Nosto’s CDN. If you very recently implemented Nosto  occasionally errors might occur and re-indexation of images is needed.

In any case, please review markup-example in tagging guide and then use the Nosto debug-toolbar and check that the image url mapped to Nosto is correct.

If image tagging is correct, there might have been an unusual error with Nosto’s image processing. In this case a manual reindex of product details is required which is launched from the Nosto admin.

In case Nosto is implemented on a site unaccessible from the Internet, this is expected behavior. Read about implementing Nosto on test environments.

### New Product Image Doesn’t Update

If you update a new image version for an old product, preferably save the image with a new file name, which typically generates a new image url for the product image as well.

Nosto doesn’t recognize images by file size or image content but by URL, hence if a url for an image is unaltered, Nosto doesn’t update image version in it’s index and uses older version of the product image in recommendations. On the contrary, new products and product images are automatically fetched and processed and no manual input is needed.

Manual re-index for an image version can be launched by enabling debug-mode and clicking  Send Product Update Request-button on a product page.

For a full re-index, log in to Nosto’s admin and navigate to Tools > Products > Update products.

### Unsharp Images

Typically when an image in a recommendation looks unsharp it’s simply because a small image version is upscaled to a bigger physical area in the template. Applying a bigger image version in a recommendation template will fix the issue, so essentially you simply need to adjust the styling slightly and use a bigger image version stored in Nosto’s CDN

When creating a recommendation template on Nosto Admin Panel, you can choose which image size you would like to use. Nosto doesn’t improve or manipulate pictures, however image versions 1-6 are cropped into squares, sharpened a bit, and zoomed slightly closer to details. Two versions, seven and eight, are original versions, but with aspect limitations of 200px (7) and 400px (8) respectively.

### Increased Load Time or Page Size

In the event of slow recommendation load time or increased page size, most likely a Nosto recommendation either uses an original product image or too big image version, which are downscaled to fit a smaller physical area in a browser, making it an opposite scenario to the unsharp images, with different consequences.

Debug by reviewing that the template uses a suitable image version and not an original product image as high-resolution image versions can often exceed size of 1M, consequently slowing down the recommendation load time if multiple products are displayed in a recommendation.

### Vertical and Horizontal Product Images

Nosto doesn’t recognize details or tell the difference between horizontal and vertical images, so be sure that most of the details of a product  are in the center of the picture. Alternatively you might need to adjust the implementation slightly so that you would consistently map either horizontal or vertical version.