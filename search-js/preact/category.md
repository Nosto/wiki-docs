# Preact Category Components

The category module provides specialized components for category-based search, navigation, and filtering. These components enable users to browse products by categories, subcategories, and hierarchical taxonomies with intuitive navigation patterns.

## Installation

```bash
npm install @nosto/search-js
```

## Basic Usage

```tsx
import { CategoryBrowser, CategoryFilter } from '@nosto/search-js/preact/category';
import { SearchProvider } from '@nosto/search-js/preact/common';

const CategoryPage = () => (
  <SearchProvider config={{ accountId: 'your-account-id' }}>
    <div className="category-page">
      <CategoryBrowser />
      <CategoryFilter />
    </div>
  </SearchProvider>
);
```

## Core Components

### CategoryBrowser

Main navigation component for browsing product categories.

```tsx
import { CategoryBrowser } from '@nosto/search-js/preact/category';

<CategoryBrowser
  layout="grid" // "list", "tree", "megamenu"
  showImages={true}
  showCounts={true}
  showHierarchy={true}
  maxDepth={3}
  expandable={true}
  searchable={true}
  onCategorySelect={(category) => navigateToCategory(category)}
  renderCategory={(category, level) => (
    <div className={`category-item level-${level}`}>
      <img src={category.imageUrl} alt={category.name} />
      <h3>{category.name}</h3>
      <span className="product-count">({category.productCount})</span>
    </div>
  )}
  renderEmpty={() => (
    <div className="empty-categories">
      <p>No categories available</p>
    </div>
  )}
/>
```

### CategoryTree

Hierarchical tree view for category navigation.

```tsx
import { CategoryTree } from '@nosto/search-js/preact/category';

<CategoryTree
  categories={categories}
  expandedKeys={expandedCategories}
  selectedKeys={selectedCategories}
  checkable={false}
  draggable={false}
  showIcon={true}
  showLine={true}
  defaultExpandAll={false}
  onExpand={(expandedKeys) => setExpandedCategories(expandedKeys)}
  onSelect={(selectedKeys) => setSelectedCategories(selectedKeys)}
  renderTitle={(category) => (
    <span className="category-title">
      {category.name}
      <span className="count">({category.productCount})</span>
    </span>
  )}
  renderIcon={(category) => (
    category.hasChildren ? <FolderIcon /> : <ProductIcon />
  )}
/>
```

### CategoryFilter

Filter component for category-based product filtering.

```tsx
import { CategoryFilter } from '@nosto/search-js/preact/category';

<CategoryFilter
  title="Categories"
  collapsible={true}
  multiple={true}
  showSearch={true}
  showCounts={true}
  maxVisible={8}
  sortBy="count" // "name", "alphabetical"
  sortOrder="desc"
  renderCategory={(category, selected) => (
    <label className="category-filter-item">
      <input 
        type="checkbox"
        checked={selected}
        onChange={() => toggleCategory(category.id)}
      />
      <span className="category-name">{category.name}</span>
      <span className="category-count">({category.productCount})</span>
    </label>
  )}
  onSelectionChange={(selectedCategories) => {
    updateSearchFilters({ categories: selectedCategories });
  }}
/>
```

### Breadcrumbs

Navigation breadcrumbs showing category hierarchy.

```tsx
import { Breadcrumbs } from '@nosto/search-js/preact/category';

<Breadcrumbs
  path={currentCategoryPath}
  separator=">"
  maxItems={5}
  showHome={true}
  homeText="Home"
  renderItem={(item, isLast) => (
    <span className={`breadcrumb-item ${isLast ? 'active' : ''}`}>
      {isLast ? (
        item.name
      ) : (
        <a href={item.url}>{item.name}</a>
      )}
    </span>
  )}
  onItemClick={(item) => navigateToCategory(item)}
/>
```

## Advanced Components

### MegaMenu

Rich mega menu for category navigation.

```tsx
import { MegaMenu } from '@nosto/search-js/preact/category';

<MegaMenu
  categories={topLevelCategories}
  columns={4}
  showImages={true}
  showPromotions={true}
  maxSubcategories={8}
  delay={200}
  renderCategory={(category) => (
    <div className="mega-menu-category">
      <h3>
        <a href={category.url}>{category.name}</a>
      </h3>
      <ul className="subcategories">
        {category.children.map(subcategory => (
          <li key={subcategory.id}>
            <a href={subcategory.url}>{subcategory.name}</a>
          </li>
        ))}
      </ul>
    </div>
  )}
  renderPromotion={(promotion) => (
    <div className="mega-menu-promo">
      <img src={promotion.imageUrl} alt={promotion.title} />
      <h4>{promotion.title}</h4>
      <p>{promotion.description}</p>
      <a href={promotion.url} className="promo-cta">
        {promotion.ctaText}
      </a>
    </div>
  )}
/>
```

### CategoryCard

Individual category display card.

```tsx
import { CategoryCard } from '@nosto/search-js/preact/category';

<CategoryCard
  category={category}
  layout="standard" // "compact", "featured", "minimal"
  showImage={true}
  showDescription={true}
  showProductCount={true}
  showSubcategories={true}
  imageAspectRatio="4:3"
  onClick={(category) => navigateToCategory(category)}
  renderBadge={(category) => (
    category.featured && <span className="featured-badge">Featured</span>
  )}
/>
```

### CategorySlider

Horizontal scrolling category browser.

```tsx
import { CategorySlider } from '@nosto/search-js/preact/category';

<CategorySlider
  categories={categories}
  itemsToShow={6}
  itemsToScroll={3}
  infinite={true}
  autoplay={false}
  arrows={true}
  dots={false}
  responsive={[
    { breakpoint: 768, settings: { itemsToShow: 3 } },
    { breakpoint: 480, settings: { itemsToShow: 2 } }
  ]}
  renderCategory={(category) => (
    <CategoryCard category={category} layout="compact" />
  )}
/>
```

## Search Integration

### CategorySearch

Search within specific categories.

```tsx
import { CategorySearch } from '@nosto/search-js/preact/category';

<CategorySearch
  categoryId={currentCategory.id}
  placeholder={`Search in ${currentCategory.name}...`}
  showCategoryContext={true}
  showSubcategories={true}
  onSearch={(query, categoryId) => {
    performCategorySearch(query, categoryId);
  }}
  renderCategoryContext={(category) => (
    <div className="search-context">
      <span>Searching in: </span>
      <strong>{category.name}</strong>
      <button onClick={() => searchAllCategories()}>
        Search all categories
      </button>
    </div>
  )}
/>
```

### CategoryResults

Display search results within category context.

```tsx
import { CategoryResults } from '@nosto/search-js/preact/category';

<CategoryResults
  categoryId={currentCategory.id}
  showCategoryInfo={true}
  showSubcategoryResults={true}
  showRelatedCategories={true}
  renderCategoryHeader={(category) => (
    <div className="category-header">
      <img src={category.imageUrl} alt={category.name} />
      <div>
        <h1>{category.name}</h1>
        <p>{category.description}</p>
        <div className="category-stats">
          <span>{category.productCount} products</span>
          <span>{category.subcategoryCount} subcategories</span>
        </div>
      </div>
    </div>
  )}
  renderSubcategoryResults={(subcategory, products) => (
    <div className="subcategory-section">
      <h3>{subcategory.name}</h3>
      <ProductGrid products={products} />
      <a href={subcategory.url}>View all in {subcategory.name}</a>
    </div>
  )}
/>
```

## Mobile Components

### MobileCategoryMenu

Mobile-optimized category navigation.

```tsx
import { MobileCategoryMenu } from '@nosto/search-js/preact/category';

<MobileCategoryMenu
  categories={categories}
  visible={menuVisible}
  onClose={() => setMenuVisible(false)}
  searchable={true}
  showBackButton={true}
  animationType="slide" // "fade", "none"
  renderHeader={(currentCategory) => (
    <div className="mobile-menu-header">
      <button onClick={goBack}>← Back</button>
      <h2>{currentCategory?.name || 'Categories'}</h2>
      <button onClick={() => setMenuVisible(false)}>✕</button>
    </div>
  )}
  renderCategory={(category, hasChildren) => (
    <div className="mobile-category-item">
      <span onClick={() => selectCategory(category)}>
        {category.name}
      </span>
      {hasChildren && (
        <button onClick={() => expandCategory(category)}>
          →
        </button>
      )}
    </div>
  )}
/>
```

### CategoryTabs

Tab-based category navigation for mobile.

```tsx
import { CategoryTabs } from '@nosto/search-js/preact/category';

<CategoryTabs
  categories={topLevelCategories}
  activeTab={activeCategory}
  scrollable={true}
  centered={false}
  showMore={true}
  moreText="More"
  onChange={(category) => setActiveCategory(category)}
  renderTab={(category, active) => (
    <div className={`category-tab ${active ? 'active' : ''}`}>
      <img src={category.iconUrl} alt="" />
      <span>{category.name}</span>
    </div>
  )}
  renderTabPanel={(category) => (
    <CategoryTabContent category={category} />
  )}
/>
```

## Configuration and Hooks

### useCategoryNavigation

Hook for category navigation logic.

```tsx
import { useCategoryNavigation } from '@nosto/search-js/preact/category';

const CategoryComponent = () => {
  const {
    categories,
    currentCategory,
    breadcrumbs,
    subcategories,
    navigateToCategory,
    goBack,
    goHome,
    isLoading,
    error
  } = useCategoryNavigation({
    categoryId: 'electronics',
    loadSubcategories: true,
    maxDepth: 3
  });

  return (
    <div>
      <Breadcrumbs path={breadcrumbs} onNavigate={navigateToCategory} />
      <CategoryTree 
        categories={subcategories}
        onSelect={navigateToCategory}
      />
    </div>
  );
};
```

### useCategoryFilter

Hook for category-based filtering.

```tsx
import { useCategoryFilter } from '@nosto/search-js/preact/category';

const FilterComponent = () => {
  const {
    availableCategories,
    selectedCategories,
    selectCategory,
    deselectCategory,
    toggleCategory,
    clearCategories,
    isSelected,
    getSelectedCount
  } = useCategoryFilter();

  return (
    <div>
      <h3>Categories ({getSelectedCount()})</h3>
      {availableCategories.map(category => (
        <label key={category.id}>
          <input
            type="checkbox"
            checked={isSelected(category.id)}
            onChange={() => toggleCategory(category.id)}
          />
          {category.name} ({category.productCount})
        </label>
      ))}
      <button onClick={clearCategories}>Clear All</button>
    </div>
  );
};
```

## Styling and Theming

### CSS Custom Properties

```css
:root {
  --category-card-background: #ffffff;
  --category-card-border: #e1e5e9;
  --category-card-border-radius: 8px;
  --category-card-shadow: 0 2px 4px rgba(0,0,0,0.1);
  --category-card-hover-shadow: 0 4px 8px rgba(0,0,0,0.15);
  
  --category-tree-indent: 24px;
  --category-tree-line-color: #dee2e6;
  --category-tree-icon-size: 16px;
  
  --breadcrumb-separator-color: #6c757d;
  --breadcrumb-link-color: #007bff;
  --breadcrumb-active-color: #495057;
  
  --mega-menu-background: #ffffff;
  --mega-menu-border: #e1e5e9;
  --mega-menu-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
```

### Component Classes

```css
.nosto-category-browser {
  /* Category browser container */
}

.nosto-category-card {
  /* Individual category card */
}

.nosto-category-tree {
  /* Tree view container */
}

.nosto-category-filter {
  /* Filter component */
}

.nosto-breadcrumbs {
  /* Breadcrumb navigation */
}

.nosto-mega-menu {
  /* Mega menu container */
}
```

## API Reference

### CategoryBrowser Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `string` | 'grid' | Display layout |
| `showImages` | `boolean` | true | Show category images |
| `showCounts` | `boolean` | true | Show product counts |
| `showHierarchy` | `boolean` | true | Show category hierarchy |
| `maxDepth` | `number` | 3 | Maximum nesting depth |
| `onCategorySelect` | `function` | - | Category selection handler |

### CategoryTree Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `categories` | `Category[]` | - | Category data |
| `expandedKeys` | `string[]` | [] | Expanded category IDs |
| `selectedKeys` | `string[]` | [] | Selected category IDs |
| `checkable` | `boolean` | false | Show checkboxes |
| `showIcon` | `boolean` | true | Show category icons |

### CategoryFilter Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `title` | `string` | 'Categories' | Filter section title |
| `multiple` | `boolean` | true | Allow multiple selections |
| `showSearch` | `boolean` | true | Show search input |
| `maxVisible` | `number` | 8 | Max visible categories |
| `onSelectionChange` | `function` | - | Selection change handler |

## Data Structure

### Category Interface

```typescript
interface Category {
  id: string;
  name: string;
  slug: string;
  url: string;
  description?: string;
  imageUrl?: string;
  iconUrl?: string;
  productCount: number;
  subcategoryCount: number;
  level: number;
  parentId?: string;
  children?: Category[];
  featured?: boolean;
  hidden?: boolean;
  metadata?: Record<string, any>;
}
```

### CategoryPath Interface

```typescript
interface CategoryPath {
  categories: Category[];
  currentCategory: Category;
  breadcrumbs: Breadcrumb[];
}

interface Breadcrumb {
  id: string;
  name: string;
  url: string;
  level: number;
}
```

## Accessibility

Category components follow WCAG 2.1 AA guidelines:

- **Keyboard Navigation**: Full keyboard support for all interactive elements
- **Screen Reader Support**: Proper ARIA labels and navigation structure
- **Focus Management**: Logical focus order and visible focus indicators
- **Hierarchical Structure**: Proper heading levels and landmark roles

```tsx
<CategoryBrowser
  accessibilityProps={{
    role: 'navigation',
    ariaLabel: 'Product categories',
    keyboardNavigation: true,
    announceSelection: true
  }}
/>
```

## Performance

### Lazy Loading

```tsx
<CategoryBrowser
  lazyLoad={true}
  loadOnExpand={true}
  virtualScrolling={true}
  cacheCategories={true}
  preloadDepth={1}
/>
```

### Optimization

```tsx
import { memo } from 'preact/compat';

const OptimizedCategoryCard = memo(CategoryCard, (prevProps, nextProps) => {
  return prevProps.category.id === nextProps.category.id &&
         prevProps.category.productCount === nextProps.category.productCount;
});
```