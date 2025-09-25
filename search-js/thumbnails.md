# Thumbnails Module

The thumbnails module provides powerful image resizing and optimization capabilities for product images in search results. It supports multiple platforms including Shopify, Nosto, and generic image services, with automatic optimization for different screen sizes and devices.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```typescript
import { 
  nostoThumbnailDecorator, 
  shopifyThumbnailDecorator,
  generateThumbnailUrl 
} from '@nosto/search-js/thumbnails';

// Apply Nosto thumbnail optimization
const nostoDecorator = nostoThumbnailDecorator({ width: 300, height: 300 });
const optimizedResults = nostoDecorator(searchResults);

// Apply Shopify thumbnail optimization
const shopifyDecorator = shopifyThumbnailDecorator({ size: '300x300' });
const shopifyResults = shopifyDecorator(searchResults);
```

## Key Features

### 🖼️ Multi-Platform Support

Support for various image hosting platforms with platform-specific optimizations.

```typescript
import { 
  nostoThumbnailDecorator,
  shopifyThumbnailDecorator,
  thumbnailDecorator 
} from '@nosto/search-js/thumbnails';

// Nosto image optimization
const nostoDecorator = nostoThumbnailDecorator({
  width: 400,
  height: 400,
  quality: 80,
  format: 'webp'
});

// Shopify image optimization
const shopifyDecorator = shopifyThumbnailDecorator({
  size: '400x400',
  crop: 'center',
  format: 'webp'
});

// Generic thumbnail decorator
const genericDecorator = thumbnailDecorator({
  width: 400,
  height: 400,
  mode: 'crop'
});
```

### 📱 Responsive Images

Generate multiple image sizes for responsive design and different device types.

```typescript
import { nostoThumbnailDecorator } from '@nosto/search-js/thumbnails';

const responsiveDecorator = nostoThumbnailDecorator({
  responsive: true,
  sizes: [
    { width: 150, height: 150, name: 'thumbnail' },
    { width: 300, height: 300, name: 'small' },
    { width: 600, height: 600, name: 'medium' },
    { width: 1200, height: 1200, name: 'large' }
  ]
});

const results = responsiveDecorator(searchResults);
// Each result will have multiple image URLs for different sizes
```

### ⚡ Performance Optimization

Automatic format optimization and lazy loading support for better performance.

```typescript
import { nostoThumbnailDecorator } from '@nosto/search-js/thumbnails';

const optimizedDecorator = nostoThumbnailDecorator({
  width: 300,
  height: 300,
  quality: 85,
  format: 'auto', // Automatically choose best format (WebP, AVIF, etc.)
  progressive: true,
  lazyLoad: true
});
```

## API Reference

### nostoThumbnailDecorator(options: NostoThumbnailOptions): Decorator

Creates a decorator for Nosto-hosted images with advanced optimization features.

**Parameters:**

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `width` | `number` | No | 300 | Image width in pixels |
| `height` | `number` | No | 300 | Image height in pixels |
| `quality` | `number` | No | 80 | Image quality (1-100) |
| `format` | `string` | No | 'auto' | Image format (webp, jpg, png, auto) |
| `crop` | `string` | No | 'center' | Crop mode (center, top, bottom, left, right) |
| `progressive` | `boolean` | No | `false` | Enable progressive JPEG |
| `responsive` | `boolean` | No | `false` | Generate responsive image sizes |
| `sizes` | `ImageSize[]` | No | - | Custom responsive sizes |

### shopifyThumbnailDecorator(options: ShopifyThumbnailOptions): Decorator

Creates a decorator for Shopify-hosted images using Shopify's image transformation API.

**Parameters:**

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `size` | `string` | No | '300x300' | Image size (e.g., '300x300', '150x', 'x200') |
| `crop` | `string` | No | 'center' | Crop position |
| `scale` | `number` | No | 1 | Image scale factor (1, 2, 3) |
| `format` | `string` | No | 'jpg' | Image format |

### thumbnailDecorator(options: ThumbnailOptions): Decorator

Generic thumbnail decorator for custom image services.

**Parameters:**

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `width` | `number` | Yes | - | Target width |
| `height` | `number` | Yes | - | Target height |
| `mode` | `string` | No | 'fit' | Resize mode (fit, crop, fill) |
| `urlTemplate` | `string` | No | - | Custom URL template |

### generateThumbnailUrl(imageUrl: string, options: ThumbnailOptions): string

Generates optimized thumbnail URLs for any image.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `imageUrl` | `string` | Yes | Original image URL |
| `options` | `ThumbnailOptions` | Yes | Thumbnail configuration |

## Supported Formats

### Nosto Platform

- **Formats**: WebP, AVIF, JPEG, PNG
- **Features**: Quality control, progressive loading, responsive images
- **Max Size**: 2000x2000 pixels
- **Crop Modes**: center, top, bottom, left, right, smart

### Shopify Platform

- **Formats**: JPEG, PNG, WebP
- **Sizes**: Predefined sizes (50x50 to 2048x2048)
- **Features**: High-DPI support, automatic optimization
- **Crop Modes**: center, top, bottom, left, right

### Generic Platform

- **Flexible URL templating**
- **Custom transformation parameters**
- **Support for any image service**

## Examples

### Basic Thumbnail Generation

```typescript
import { nostoThumbnailDecorator } from '@nosto/search-js/thumbnails';

const decorator = nostoThumbnailDecorator({
  width: 250,
  height: 250,
  quality: 85
});

const products = [
  { 
    id: '1', 
    name: 'Product 1', 
    imageUrl: 'https://images.nosto.com/product1.jpg' 
  }
];

const enhanced = decorator(products);
// Result includes optimized thumbnailUrl
```

### Shopify Integration

```typescript
import { shopifyThumbnailDecorator } from '@nosto/search-js/thumbnails';

const shopifyDecorator = shopifyThumbnailDecorator({
  size: '300x300',
  crop: 'center',
  scale: 2 // For retina displays
});

const shopifyProducts = [
  {
    id: '1',
    name: 'Shopify Product',
    imageUrl: 'https://cdn.shopify.com/s/files/1/0001/product.jpg'
  }
];

const optimized = shopifyDecorator(shopifyProducts);
```

### Responsive Image Generation

```typescript
import { nostoThumbnailDecorator } from '@nosto/search-js/thumbnails';

const responsiveDecorator = nostoThumbnailDecorator({
  responsive: true,
  sizes: [
    { width: 150, height: 150, name: 'thumb' },
    { width: 300, height: 300, name: 'small' },
    { width: 600, height: 600, name: 'medium' },
    { width: 1200, height: 1200, name: 'large' }
  ]
});

const products = responsiveDecorator(searchResults);

// Access different sizes
products.forEach(product => {
  console.log('Thumbnail:', product.images?.thumb);
  console.log('Small:', product.images?.small);
  console.log('Medium:', product.images?.medium);
  console.log('Large:', product.images?.large);
});
```

### Custom URL Template

```typescript
import { thumbnailDecorator } from '@nosto/search-js/thumbnails';

const customDecorator = thumbnailDecorator({
  width: 300,
  height: 300,
  mode: 'crop',
  urlTemplate: 'https://mycdn.com/resize/{width}x{height}/{quality}/{url}'
});
```

### Integration with Search Results

```typescript
import { search, applyDecorators } from '@nosto/search-js/core';
import { nostoThumbnailDecorator } from '@nosto/search-js/thumbnails';
import { priceDecorator } from '@nosto/search-js/currencies';

const enhancedSearch = async (query: string) => {
  const results = await search({ query });
  
  const decorators = [
    nostoThumbnailDecorator({
      width: 300,
      height: 300,
      quality: 85,
      format: 'webp'
    }),
    priceDecorator({ currency: 'USD' })
  ];
  
  return applyDecorators(results, decorators);
};
```

### Lazy Loading Support

```typescript
import { nostoThumbnailDecorator } from '@nosto/search-js/thumbnails';

const lazyDecorator = nostoThumbnailDecorator({
  width: 300,
  height: 300,
  lazyLoad: true,
  placeholder: {
    width: 300,
    height: 300,
    quality: 20,
    blur: 10
  }
});

const products = lazyDecorator(searchResults);
// Products will have both thumbnailUrl and placeholderUrl
```

### Multiple Image Variants

```typescript
const multiVariantDecorator = nostoThumbnailDecorator({
  variants: [
    { name: 'list', width: 200, height: 200 },
    { name: 'grid', width: 300, height: 300 },
    { name: 'detail', width: 800, height: 800 }
  ]
});

const products = multiVariantDecorator(searchResults);
// Each product has multiple image variants
```

## Performance Considerations

### Image Optimization

```typescript
// Optimized for performance
const performanceDecorator = nostoThumbnailDecorator({
  width: 300,
  height: 300,
  quality: 75, // Lower quality for faster loading
  format: 'webp', // Modern format for smaller file size
  progressive: true, // Progressive loading
  preload: 'critical' // Preload critical images
});
```

### Responsive Images with srcset

```typescript
const srcsetDecorator = nostoThumbnailDecorator({
  responsive: true,
  srcset: true,
  sizes: [
    { width: 300, density: '1x' },
    { width: 600, density: '2x' },
    { width: 900, density: '3x' }
  ]
});

// Generates srcset attribute for responsive images
const products = srcsetDecorator(searchResults);
```

## Best Practices

1. **Choose Appropriate Sizes**: Use image sizes that match your UI requirements
2. **Optimize Quality**: Balance image quality with file size (75-85% is usually optimal)
3. **Use Modern Formats**: Prefer WebP or AVIF when supported
4. **Implement Lazy Loading**: Load images only when needed
5. **Provide Placeholders**: Use low-quality placeholders for better UX
6. **Cache Aggressively**: Thumbnail URLs should be cacheable
7. **Handle Errors**: Provide fallback images for missing or broken images

## Error Handling

```typescript
import { nostoThumbnailDecorator, ThumbnailError } from '@nosto/search-js/thumbnails';

const safeDecorator = (options: NostoThumbnailOptions) => {
  try {
    return nostoThumbnailDecorator(options);
  } catch (error) {
    if (error instanceof ThumbnailError) {
      console.warn('Thumbnail generation failed:', error.message);
      // Return decorator that uses original images
      return (results) => results;
    }
    throw error;
  }
};
```

## TypeScript Support

Full TypeScript support with comprehensive type definitions:

```typescript
import type {
  NostoThumbnailOptions,
  ShopifyThumbnailOptions,
  ThumbnailOptions,
  ImageSize,
  ThumbnailDecorator
} from '@nosto/search-js/thumbnails';

const options: NostoThumbnailOptions = {
  width: 300,
  height: 300,
  quality: 85,
  format: 'webp'
};
```