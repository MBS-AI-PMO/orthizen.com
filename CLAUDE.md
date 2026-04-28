# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Shopify CLI Commands

```bash
# Start local dev server (hot-reloads on file save)
shopify theme dev --store orthizen-com.myshopify.com

# Pull live theme into current folder (overwrites local files)
shopify theme pull --live --store orthizen-com.myshopify.com

# Push local changes to live theme
shopify theme push --store orthizen-com.myshopify.com

# List themes
shopify theme list --store orthizen-com.myshopify.com
```

Local dev URL: `http://127.0.0.1:9292`  
Theme editor: `https://orthizen-com.myshopify.com/admin/themes/139286544446/editor`

---

## Brand & Design System

Reference site: **https://www.orthora-footwear.com/**  
Brand: Orthizen footwear — orthopedic shoes for nurses and service workers.

### Color Tokens

| Token | Hex | Usage |
|-------|-----|-------|
| `primary_green` | `#1eb014` | CTA buttons, sale price, checkmarks |
| `primary_blue` | `#1364b0` | Topbar, footer background, icon circles |
| `dark_navy` | `#0b1428` | Section headings |
| `mid_blue` | `#1f3b74` | USP text, feature text |
| `red_sale` | `#db0404` | Compare/crossed-out price |
| `feature_card_bg` | `#eef3fa` | Feature item card backgrounds |
| `badge_bg` | `#ececec` | Expert badge, general badges |
| `body_text` | `rgba(0,0,0,0.75)` | Body copy, descriptions |

### Typography

Font: **Lato** (Google Fonts — weights 300–900)

| Element | Desktop | Mobile | Weight |
|---------|---------|--------|--------|
| Hero H1 | 40px / lh 50px | 28px / lh 40px | 600 |
| Section H2 | 34px | 24px / lh 30px | 600 |
| Collection H2 | 32px | 24px | 700 |
| Feature H3 | 18px / lh 26px | 16px | 700 |
| Body | 16px / lh 28.8px | 14px | 400 |
| Body color | `rgba(0,0,0,0.75)` | — | — |

Letter spacing: `0.6px` on navigation and CTA buttons.

### Button / CTA Style

- Background: `#1eb014` (green) or `#1f8015` (darker green)
- Text: `#ffffff`, weight 500, size 16px
- Border radius: `6px`
- Padding: `10px 20px`
- Height: `49px`
- Letter spacing: `0.6px`
- Full width on mobile (`100%`), fixed width on desktop (`500px` for hero CTA)

### USP / Feature Bar Items

Three inline items: **12 Hours Of Comfort · Arch Support · Wide Toe Fit**

```
icon_bg_color: #1364b0   (blue circle)
text_color:    #1f3b74   (mid blue)
text_font_size: 14px
text_font_weight: 600
```

Icons: `clock`, `arch`, `toe` (SVG icons in the theme's icon system)

---

## Homepage Page Structure

Section order in `templates/index.json`:

1. **topbar-slider** — Mobile animated announcement bar (blue bg `#1364b0`, white text)
2. **topbar-desktop** — Desktop 3-column topbar (clearance, guarantee, reviews)
3. **header** — Sticky header, logo top-left, shows cart/account/search/social icons
4. **hero-banner** (`main-custom-image-banner`) — Image right layout, text left. Heading + subtext + green CTA + "45-day Comfort Guarantee" badge + 3 USP items
5. **image-with-features** (`image-with-list`) — Image left (57% width), 5 feature cards right on `#eef3fa` bg. Section heading centered above.
6. **featured-product** — Single product spotlight with variant selector, ATC button (green), ShopPay note
7. **featured-collection-1** — "Find The Perfect Shoe For You" — 4 products, 4 cols desktop / 2 cols mobile
8. **foot-conditions** (`multicolumn`) — 3-column conditions grid (General Relief, Plantar Fasciitis, Bunions)
9. **featured-collection-2** — Women's footwear collection, same card layout as collection-1
10. **foot-pain-section** (`foot-problem-solve`) — Pain checklist (2 cols, 10 items), 2 images right
11. **footer** — Blue bg `#1364b0`, 5-col desktop: brand copy + newsletter + nav + info + trust badges

### Footer Details

- Background: `#1364b0`, text `rgba(255,255,255,0.75)`
- 4 trust badges: 45-Day Guarantee · Free Shipping · Secure Payment · 500k+ Customers
- Navigation links: Shifts Shoes, Clearance Sale, Women's, Men's, Accessories, Contact, Track Order
- Copyright: `© 2026, Orthora Footwear`
- Payment icons: Apple Pay, Google Pay, ShopPay, Visa, Mastercard, Amex, PayPal
- Support email: `support@orthora-footwear.com`

---

## Architecture Overview

This is a Shopify Online Store 2.0 theme named **Keton** by ThemeOcean. It uses a custom **XO web component library** as its UI foundation.

### Core Files

| File | Purpose |
|------|---------|
| `layout/theme.liquid` | Master HTML shell — loads all global CSS/JS |
| `assets/xo-webcomponents.js` | 770KB web component library (primary UI framework) |
| `assets/main.css` | 522KB compiled theme styles |
| `snippets/settings-adapter.liquid` | Compiles color schemes into CSS custom properties per-page |

### XO Web Components

- `<xo-container>` — responsive width container
- `<xo-grid>` — CSS grid; columns via `--xs`, `--sm`, `--md`, `--lg`, `--xxl`; position via `--start-md`, `--start-xxl`
- `<xo-animate xo-type="fade-up" xo-order="1" xo-duration="700">` — scroll-triggered animation
- `<xo-animate xo-cascade>` — cascades animation order to children
- `<xo-carousel>` / `<xo-carousel-inner>` / `<xo-carousel-list>` / `<xo-carousel-slide>` — carousel
- `<xo-carousel-pagination>`, `<xo-carousel-prev>`, `<xo-carousel-next>` — carousel controls
- `<xo-sticky>` — sticky positioning
- `<xo-video-cover>` — background video

### Section Pattern

```liquid
{% liquid
  assign padding_top = '--pt: ' | append: section.settings.padding_top | append: ';'
  assign padding_bottom = '--pb: ' | append: section.settings.padding_bottom | append: ';'
%}

<section style='{{ padding_top }} {{ padding_bottom }}' class='my-section'>
  <xo-container {% render 'section-layout' %}>
    <xo-grid>
      <div style='--xs: 12; --md: 6'>...</div>
    </xo-grid>
  </xo-container>
</section>

<style>/* scoped styles */</style>

{% schema %}
{ "name": "...", "settings": [...], "blocks": [...], "presets": [...] }
{% endschema %}
```

Helpers inside `<xo-container>`:
- `{% render 'section-layout' %}` — width/fullwidth from settings
- `{% render 'section-bg-image' %}` — background image/video/overlay
- `{% render 'section-attr' %}` — background type + color scheme attributes

### Template & Settings Data

**Important:** Schema `default` values are not always applied in local dev. When adding new section settings, explicitly seed their values in the relevant `templates/*.json` section's `"settings"` object.

### Snippet Conventions

- `{% render 'button', text: ..., link: ..., mode: 'dark', style: 'secondary', size: 'sm', radius: ..., padding: ... %}` — always use for CTAs
- `{% render 'icon', name: 'arrow-left2', icon_size: '15' %}` — SVG icons
- `{% render 'product-card-styles' %}` — product card; variant set by section setting
- Schema labels use `t:sections.*` translation keys — check `locales/en.default.json` before adding new ones

### Responsive Grid

```html
<div style='--xs: 12; --md: 6; --start-md: 2; --lg: 4'>
```

`--xs` through `--xxl` = column span; `--start-*` = grid column start.

### Section Naming Convention

Variants use `-v1`, `-v2` … suffix (e.g. `banner-v1` through `banner-v11`). Follow this when creating new sections.
