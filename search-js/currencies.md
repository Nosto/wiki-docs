# Currencies Module

The currencies module provides utilities for formatting monetary values and decorating search results with properly formatted price information. It handles multi-currency scenarios and integrates with Nosto's currency settings for consistent price display across your application.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```typescript
import { priceDecorator, getCurrencyFormatting } from '@nosto/search-js/currencies';

// Apply price formatting to search results
const decoratedResults = priceDecorator({ currency: 'USD' })(searchResults);

// Get currency formatting options
const formatting = getCurrencyFormatting('EUR');
```

## Key Features

### 💰 Price Formatting

Automatically formats prices according to currency-specific rules and locale conventions.

```typescript
import { getCurrencyFormatting } from '@nosto/search-js/currencies';

// Get formatting configuration for different currencies
const usdFormatting = getCurrencyFormatting('USD');
// Returns: { symbol: '$', position: 'before', decimalPlaces: 2, ... }

const eurFormatting = getCurrencyFormatting('EUR');
// Returns: { symbol: '€', position: 'after', decimalPlaces: 2, ... }

const jpyFormatting = getCurrencyFormatting('JPY');
// Returns: { symbol: '¥', position: 'before', decimalPlaces: 0, ... }
```

### 🎨 Price Decorator

Enhances search results with formatted price information using the decorator pattern.

```typescript
import { priceDecorator } from '@nosto/search-js/currencies';
import { search, applyDecorators } from '@nosto/search-js/core';

const searchResults = await search({ query: 'shoes' });

// Basic price decoration
const basicDecorator = priceDecorator({ currency: 'USD' });
const formattedResults = basicDecorator(searchResults);

// Advanced price decoration with custom options
const advancedDecorator = priceDecorator({
  currency: 'EUR',
  showCurrency: true,
  precision: 2,
  locale: 'de-DE'
});

const decoratedResults = applyDecorators(searchResults, [advancedDecorator]);
```

### 🌍 Multi-Currency Support

Handle multiple currencies simultaneously with automatic detection and formatting.

```typescript
import { priceDecorator } from '@nosto/search-js/currencies';

// Multi-currency decorator that adapts based on product currency
const multiCurrencyDecorator = priceDecorator({
  detectCurrency: true,
  fallbackCurrency: 'USD',
  showOriginalCurrency: true
});

const results = multiCurrencyDecorator(searchResults);
```

## API Reference

### getCurrencyFormatting(currency: string): CurrencyFormat

Retrieves formatting configuration for a specific currency.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `currency` | `string` | Yes | ISO 4217 currency code (e.g., 'USD', 'EUR') |

**Returns:** `CurrencyFormat`

```typescript
interface CurrencyFormat {
  symbol: string;           // Currency symbol ($, €, ¥)
  position: 'before' | 'after'; // Symbol position relative to amount
  decimalPlaces: number;    // Number of decimal places
  thousandsSeparator: string; // Thousands separator character
  decimalSeparator: string; // Decimal separator character
  code: string;            // ISO currency code
}
```

### priceDecorator(options: PriceDecoratorOptions): Decorator

Creates a decorator function that formats prices in search results.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `options` | `PriceDecoratorOptions` | Yes | Price formatting configuration |

**PriceDecoratorOptions:**

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `currency` | `string` | No | 'USD' | Target currency code |
| `showCurrency` | `boolean` | No | `true` | Whether to show currency symbol |
| `precision` | `number` | No | `2` | Number of decimal places |
| `locale` | `string` | No | 'en-US' | Locale for number formatting |
| `detectCurrency` | `boolean` | No | `false` | Auto-detect currency from product data |
| `fallbackCurrency` | `string` | No | 'USD' | Fallback currency when detection fails |
| `customFormat` | `PriceFormat` | No | - | Custom formatting override |

**Returns:** `Decorator` function that can be applied to search results.

## Supported Currencies

The module supports all major currencies with proper formatting rules:

| Currency | Code | Symbol | Decimal Places | Example |
|----------|------|--------|----------------|---------|
| US Dollar | USD | $ | 2 | $19.99 |
| Euro | EUR | € | 2 | 19,99 € |
| British Pound | GBP | £ | 2 | £19.99 |
| Japanese Yen | JPY | ¥ | 0 | ¥1999 |
| Canadian Dollar | CAD | C$ | 2 | C$19.99 |
| Australian Dollar | AUD | A$ | 2 | A$19.99 |
| Swiss Franc | CHF | CHF | 2 | CHF 19.99 |
| Chinese Yuan | CNY | ¥ | 2 | ¥19.99 |
| Indian Rupee | INR | ₹ | 2 | ₹19.99 |
| Brazilian Real | BRL | R$ | 2 | R$ 19,99 |

## Examples

### Basic Price Formatting

```typescript
import { priceDecorator } from '@nosto/search-js/currencies';

// Format prices in US Dollars
const usdDecorator = priceDecorator({ 
  currency: 'USD',
  showCurrency: true 
});

const products = [
  { id: '1', name: 'Shoes', price: 99.99 },
  { id: '2', name: 'Shirt', price: 29.50 }
];

const formattedProducts = usdDecorator(products);
// Result: [
//   { id: '1', name: 'Shoes', price: 99.99, formattedPrice: '$99.99' },
//   { id: '2', name: 'Shirt', price: 29.50, formattedPrice: '$29.50' }
// ]
```

### European Currency Formatting

```typescript
// Format prices in Euros with German locale
const eurDecorator = priceDecorator({
  currency: 'EUR',
  locale: 'de-DE',
  showCurrency: true
});

const formattedProducts = eurDecorator(products);
// Result: Prices formatted as "99,99 €"
```

### Multi-Currency Products

```typescript
// Handle products with different currencies
const multiCurrencyDecorator = priceDecorator({
  detectCurrency: true,
  showOriginalCurrency: true,
  fallbackCurrency: 'USD'
});

const mixedProducts = [
  { id: '1', name: 'US Product', price: 99.99, currency: 'USD' },
  { id: '2', name: 'EU Product', price: 89.99, currency: 'EUR' },
  { id: '3', name: 'No Currency', price: 49.99 } // Uses fallback
];

const formatted = multiCurrencyDecorator(mixedProducts);
```

### Custom Price Formatting

```typescript
import { priceDecorator, CurrencyFormat } from '@nosto/search-js/currencies';

// Define custom formatting rules
const customFormat: CurrencyFormat = {
  symbol: 'Kr',
  position: 'after',
  decimalPlaces: 2,
  thousandsSeparator: ' ',
  decimalSeparator: ',',
  code: 'SEK'
};

const customDecorator = priceDecorator({
  currency: 'SEK',
  customFormat: customFormat
});
```

### Integration with Search

```typescript
import { search, applyDecorators } from '@nosto/search-js/core';
import { priceDecorator } from '@nosto/search-js/currencies';

const performSearch = async (query: string, userCurrency: string) => {
  const results = await search({ query });
  
  const decorator = priceDecorator({
    currency: userCurrency,
    showCurrency: true,
    locale: getUserLocale(userCurrency)
  });
  
  return applyDecorators(results, [decorator]);
};

// Usage
const searchResults = await performSearch('laptops', 'EUR');
```

### Price Range Formatting

```typescript
const formatPriceRange = (min: number, max: number, currency: string) => {
  const decorator = priceDecorator({ currency, showCurrency: true });
  
  const mockProducts = [
    { price: min },
    { price: max }
  ];
  
  const formatted = decorator(mockProducts);
  return `${formatted[0].formattedPrice} - ${formatted[1].formattedPrice}`;
};

// Usage
const priceRange = formatPriceRange(99, 299, 'USD');
// Result: "$99.00 - $299.00"
```

## Best Practices

1. **Consistent Currency**: Use the same currency throughout the user session
2. **User Preference**: Detect user's preferred currency from location or settings
3. **Fallback Currency**: Always provide a fallback currency for missing data
4. **Locale Awareness**: Use appropriate locale settings for number formatting
5. **Performance**: Cache currency formatting configurations when possible

## Error Handling

```typescript
import { priceDecorator, CurrencyError } from '@nosto/search-js/currencies';

try {
  const decorator = priceDecorator({ currency: 'INVALID' });
} catch (error) {
  if (error instanceof CurrencyError) {
    console.error('Currency formatting error:', error.message);
    // Fallback to default currency
    const decorator = priceDecorator({ currency: 'USD' });
  }
}
```

## TypeScript Support

The currencies module is fully typed with comprehensive TypeScript definitions:

```typescript
import type {
  CurrencyFormat,
  PriceDecoratorOptions,
  FormattedProduct
} from '@nosto/search-js/currencies';

const options: PriceDecoratorOptions = {
  currency: 'EUR',
  showCurrency: true,
  precision: 2
};

const decorator = priceDecorator(options);
```