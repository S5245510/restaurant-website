# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Development
- `yarn dev` - Start the development server on http://localhost:3000
- `yarn build` - Build the production application
- `yarn start` - Start the production server

### Installation
- `yarn` - Install all dependencies

## Architecture

### Core Technology Stack
- **Next.js** with React 17 for the frontend framework
- **Tailwind CSS 2** for styling with custom color palette and Josefin Sans font
- **Shopify Storefront API** via GraphQL for eCommerce backend
- **PWA** support with next-pwa for offline functionality
- **localStorage** for cart persistence across user sessions

### Project Structure

#### API Integration (`lib/shopify.js`)
Central module for all Shopify GraphQL queries:
- `callShopify()` - Base function for GraphQL requests to Shopify Storefront API
- `getAllProductsInCollection()` - Fetches all products from configured collection (max 250)
- `getProduct()` - Fetches single product by handle
- `createCheckout()` - Creates new Shopify checkout session
- `updateCheckout()` - Updates existing checkout with new line items

#### State Management (`context/Store.js`)
Cart state managed via React Context API with three contexts:
- `CartContext` - Provides cart items, checkout URL, and loading state
- `AddToCartContext` - Handles adding items to cart
- `UpdateCartQuantityContext` - Manages quantity updates
- Automatically syncs cart data to localStorage and across browser tabs

#### Routing
- `/` - Homepage with product listings from collection
- `/products/[product]` - Dynamic product detail pages
- `/cart` - Shopping cart page

### Environment Variables Required
Must be configured in `.env.local`:
- `NEXT_PUBLIC_SHOPIFY_STORE_FRONT_ACCESS_TOKEN` - Shopify Storefront API token
- `NEXT_PUBLIC_SHOPIFY_STORE_DOMAIN` - Format: `STORE_NAME.myshopify.com`
- `NEXT_PUBLIC_SHOPIFY_COLLECTION` - Collection handle to display products from
- `NEXT_PUBLIC_LOCAL_STORAGE_NAME` - Unique key for localStorage cart data

### Configuration Files

#### Site Metadata (`next.config.js`)
Update site-wide metadata in the `env` object for SEO and branding.

#### Styling (`tailwind.config.js`)
Custom color palette defined under `colors.palette`:
- `lighter`: #F5F3FF
- `light`: #DDD6FE  
- `primary`: #5B21B6
- `dark`: #4C1D95

#### PWA Configuration
- Update `public/manifest.json` for app metadata
- Replace icons in `public/images/icons/` for different sizes

### Key Implementation Notes
- All GraphQL queries are hardcoded to fetch maximum 250 items (Shopify limit)
- No pagination implemented - if needed, use cursor-based pagination
- Cart updates trigger Shopify checkout API calls to maintain sync
- Multiple browser tabs stay synchronized via storage event listeners
- Images are served from `cdn.shopify.com` (configured in next.config.js)