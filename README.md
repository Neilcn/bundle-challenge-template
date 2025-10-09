> **This repository is provided **exclusively** for the technical assessment.**  
> Once the hiring process is complete, all of its contents may be deleted.

---

## Full brief

Find the detailed briefing, evaluation criteria, and mock-ups on Notion:  
🔗 **[Link to the exercise](https://www.notion.so/dtc-pages/Technical-Challenge-Dynamic-Product-Bundle-10-OFF-215075df3d7980809acddb8a95d6db91?source=copy_link)**

---

## How to participate

1. **Create a Shopify Partners account** (free) and set up a development store—this is where you’ll preview your work.  
2. **Fork this repository** to your personal account or organization (**Fork → Create a fork**).  
3. Work in your fork; we recommend a branch named `feature/solution`.  
4. **When you’re done**, open a **Pull Request (PR)** from your branch to `main` in *your own fork*.  
   - Leave the PR **open and unmerged** so we can review commits, diffs, and comments.  
   - In the PR **description**, paste the **theme preview link** (e.g. `https://your-dev-store.myshopify.com/?preview_theme_id=123456789`) so we can test the bundle live.  
5. Invite `@hemnys` as a *reviewer* or *collaborator* so we have access to your PR.

### Shopify Functions & Checkout UI Extensions

| What you must do | Why |
|------------------|-----|
| **Create one additional private repository** under your account (e.g. `bundle-backend`). | Keeps code that may require secrets or CI isolated from the theme fork. |
| In that repo, place **both components**:<br>• `bundle-function` (CartTransform)<br>• `bundle-checkout-extension` (UI Extension) | Centralises backend code while remaining separate from storefront assets. |
| **Open a dedicated PR to `main` for each component** (two PRs total). | Allows us to review Function and Extension independently. |
| Add **direct links** to those PRs in the README of your fork. | Fast navigation for the evaluation team. |

> **Functions or Extensions delivered without their own PRs will not be accepted.**

---


---

Good luck with the challenge!



## Implementation Overview

This project creates a bundle product functionality that allows:
- Displaying bundle products alongside main products
- Interactive bundle selection with visual feedback
- Adding both main and bundle products to cart

### Key Features Implemented

- 🎯 **Product Section**: Custom `sections/product.liquid` with bundle integration
- 🛍️ **Bundle Metafields**: Custom metafields to assign bundle products to main products
- 🎠 **Image Carousel**: Dawn theme native slider for product images
- 🌐 **Web Components**: Modern web components for interactive functionality
- 🛒 **Cart Integration**: Add main product + bundle to cart in single action

### Test Product

**Live Demo**: https://neil-bundle-app.myshopify.com/products/classic-varsity-top

This product demonstrates:
- Main product with variants
- Bundle product metafield configuration
- Interactive bundle selection
- Combined cart functionality

## Local Setup

### Prerequisites

- [Shopify CLI](https://shopify.dev/docs/themes/tools/cli/install) installed
- Node.js (v16 or higher)
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd bundle-challenge-template
   ```

2. **Connect to Shopify store**
   ```bash
   shopify theme dev --store=your-store-name.myshopify.com
   ```

3. **Login to Shopify**
   ```bash
   shopify auth login
   ```

## Shopify CLI Commands

### Development
```bash
# Start development server with live reload
shopify theme dev

# Start development server on a specific store
shopify theme dev --store=your-store-name.myshopify.com

# Start development with specific theme
shopify theme dev --theme-id=123456789
```

## Project Structure

```
bundle-challenge-template/
├── assets/
│   ├── product.js              # Bundle functionality JavaScript
│   ├── base.css               # Base styling
│   └── product.css        # Component-specific styles
├── sections/
│   └── product.liquid         # Main product section with bundle integration
├── snippets/                  # Reusable template snippets
├── templates/                 # Page templates
├── layout/                    # Theme layout files
└── README.md                 # This file
```

## Bundle Product Implementation

### 1. Product Section (`sections/product.liquid`)

The main product section includes:
- Product information display
- Bundle product integration using custom metafields
- Image carousel using Dawn's native slider
- Interactive bundle selection with web components
- Form handling for cart additions


### 2. Bundle Metafields Configuration

Custom metafields setup:
- **Namespace**: `custom`
- **Key**: `bundle_product`
- **Type**: Product reference (GID format)
- **Description**: Reference to the bundle product

### 3. Web Components (`assets/product.js`)

JavaScript functionality includes:
- Bundle card selection/deselection with visual feedback
- Form data manipulation to include bundle products
- Cart API integration for multiple product additions
- Error handling and debugging

### 4. Image Carousel Integration

Utilizes Dawn theme's native slider component:
- Product image gallery with navigation
- Bundle product image integration
- Responsive design with touch/swipe support
- Accessibility features

## Test Steps

### 1. Local Development Testing

1. **Start development server**
   ```bash
   shopify theme dev
   ```

2. **Open local preview URL** (typically `https://127.0.0.1:9292`)

3. **Navigate to test product**
   ```
   /products/classic-varsity-top
   ```

### 2. Bundle Functionality Testing

**Bundle Display Verification:**
- [ ] Bundle product card appears below main product
- [ ] Bundle product image and title display correctly
- [ ] Bundle card has proper styling and layout

**Selection Interaction:**
- [ ] Click bundle card to select (blue border appears)
- [ ] Click again to deselect (border disappears)
- [ ] Console logs show selection state changes
- [ ] Visual feedback is immediate and clear

**Cart Integration:**
- [ ] Add to cart with bundle unselected (only main product added)
- [ ] Select bundle and add to cart (both products added simultaneously)

### 3. Image Carousel Testing

**Carousel Functionality:**
- [ ] Multiple product images display in carousel
- [ ] Bundle product image appears in image grid
- [ ] Navigation controls work correctly
- [ ] Touch/swipe gestures work on mobile
- [ ] Images load properly with correct alt text

### 4. Responsive Design Testing

**Cross-Device Testing:**
- [ ] Desktop display and functionality
- [ ] Mobile responsive behavior
- [ ] Bundle card adapts to screen size
- [ ] Touch interactions work properly

## Metafield Setup Instructions

### Creating Bundle Product Metafields

1. **In Shopify Admin**, navigate to Settings > Metafields
2. **Select Products** as the resource type
3. **Add new definition**:
   - Name: `Bundle Product`
   - Namespace and key: `custom.bundle_product`
   - Type: `Product reference`
   - Description: `Reference to the bundle product for this main product`
4. **Save the definition**

### Assigning Bundle Products

1. **Navigate to Products** in Shopify Admin
2. **Select the main product** (e.g., Classic Varsity Top)
3. **Scroll to Metafields section**
4. **Select bundle product** from the product reference dropdown
5. **Save changes**


## Development Notes

### Bundle Product Resolution

The bundle product is resolved from the metafield GID using string manipulation:
```liquid
{% assign bundle_product_gid = product.metafields.custom.bundle_product.value %}
{% assign gid_parts = bundle_product_gid | split: '/' %}
{% assign bundle_product_id = gid_parts[4] %}

{% for product_item in collections.all.products %}
  {% if product_item.id == bundle_product_id %}
    {% assign bundle_product = product_item %}
    {% break %}
  {% endif %}
{% endfor %}
```

### Cart API Integration

Bundle products are added using Shopify's Cart API with the items array format:
```javascript
// Add main product
formData (from form)

// Add bundle product when selected
formData.append('items[][id]', bundleVariantId);
formData.append('items[][quantity]', '1');
```

### Error Handling

The implementation includes comprehensive error handling:
- GID parsing validation
- Product existence checks
- Variant availability verification
- Cart API error responses
- Console logging for debugging

## Troubleshooting

### Common Issues

**Bundle product not displaying:**
- Verify metafield is properly configured and assigned
- Check that bundle product is published and available
- Ensure GID parsing is working correctly (check console logs)

**Cart addition errors:**
- Verify variant IDs are correct and available
- Check network requests in browser dev tools
- Ensure Shopify Cart API is accessible

**JavaScript functionality not working:**
- Check for console errors in browser dev tools
- Verify event listeners are properly attached
- Ensure DOM elements exist before binding events

### Debug Information

Enable debugging by checking browser console for:
- Bundle product resolution logs
- GID parsing results
- Form data contents before submission
- Cart API responses
- Selection state changes

Example console output:
```
Bundle Product GID: gid://shopify/Product/9980321071395
Bundle Product ID: 9980321071395
Bundle Product Found!
Bundle Product Title: Example Bundle Product
Bundle variant added to cart: 45678901234567
```

## Backend Components

For the complete bundle functionality, additional backend components are required:

### Shopify Functions & Checkout UI Extensions

