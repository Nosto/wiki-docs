# Resizing Images

In order to use images with custom dimensions, you can use a built-in Magento re-sizer function to generate image URLs with the correct dimensions and you will need to amend the product model.

Start by reading [how to extend Nosto module](../) and [how to alter Nosto product data](./).

**Note:** In case you want a square image, only the first parameter is required.

File `app/code/My/Nosto/Observer/Product/Load.php`

```php
<?php

namespace My\Nosto\Observer\Product;

use Magento\Framework\Event\Observer;
use Magento\Framework\Event\ObserverInterface;
use Magento\Catalog\Api\ProductRepositoryInterface;

class Load implements ObserverInterface
{
    protected $imageHelper;
    protected $productRepository;

    public function __construct(
        \Magento\Catalog\Helper\Image $imageHelper,
        ProductRepositoryInterface $productRepository
    ) {
        $this->imageHelper = $imageHelper;
        $this->productRepository = $productRepository;
    }

    /**
     * Observer for "nosto_product_load_after" event
     *
     * @param Observer $observer
     * @return void
     * @throws \Magento\Framework\Exception\NoSuchEntityException
     */
    public function execute(Observer $observer)
    {
        /** @var $product \NostoProduct */
        $product = $observer->getProduct();

        $width = 200;
        $height = 300;

        $magentoProduct = $this->productRepository->getById($product->getProductId());
        $resizedImageUrl = $this->imageHelper
            ->init($magentoProduct, 'product_base_image')
            ->constrainOnly(true) // Optional
            ->keepAspectRatio(true) // Optional
            ->keepTransparency(true) // Optional
            ->keepFrame(false) // Optional
            ->resize($width, $height)
            ->getUrl();

        $product->setImageUrl($resizedImageUrl);
    }
}
```

