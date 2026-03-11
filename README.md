# 🛍️ EcomLanders Theme

A high-conversion Shopify theme built for direct-to-consumer brands. Extends a solid Online Store 2.0 base with a fully custom **Product Info Advanced** section — purpose-built for supplement, health, and DTC product pages that need social proof, urgency, and configurability without relying on third-party apps.

✅ **Drop-in on any Online Store 2.0 store** — all custom logic is self-contained in one section + two asset files.  
✅ **Zero app dependencies** — trust signals, bundle picker, media carousel, badge, and popup modal are all native Liquid + vanilla JS + CSS.  
✅ **Fully customizable via the Theme Editor** — every visual element is configurable through the Shopify customizer with no code edits needed.  
✅ **Theme-check clean** — passes Shopify's linter with only intentional suppressions documented in `.theme-check.yml`.

Shopify &nbsp; Liquid &nbsp; JavaScript ES6+ &nbsp; CSS Custom Properties

---

## ✨ Key Features

### 🎯 Product Info Advanced (`product-info-advanced`)
The flagship custom section — a conversion-focused product information layout with:

| Feature | Description |
|---|---|
| **Trust Signals** | Up to 4 configurable trust blocks with country flag images, optional text labels, pulse dot animation, icon/image support, and popup modal link behavior |
| **Open Popup / Modal** | Click any trust signal link to load the content in a modal — works with same-origin Shopify pages (full fetch + render) and external URLs (direct iframe) |
| **Media Carousel** | Scrollable image/video carousel with configurable visible cards: 1, 1.5, 2, or **2.5** on mobile |
| **Bundle Options** | Product bundle picker with configurable image fit (`cover`, `contain`, `fill`, `auto`) and aspect ratio (`1:1`, `4:5`, `auto`) |
| **Main Image Badge** | Fully customizable badge overlay: font size, font color, padding, position (4 corners), margin, and icon size — all via the customizer |
| **Benefit Icons** | Icon + text benefit list with configurable layout |
| **Subscription Toggle** | One-click subscribe & save toggle with dynamic price updates |
| **Quantity & Add to Cart** | Integrated quantity selector and ATC form using `routes.cart_add_url` |

---

## 📦 What's Included

```
EcomLanders-Theme/
├── sections/
│   └── product-info-advanced.liquid   # Custom PDP section (trust signals, bundle, badge, carousel)
├── assets/
│   ├── product-info-advanced.css      # All styles for the custom section
│   └── product-info-advanced.js       # Interactive logic (modal, carousel, bundles, subscription)
├── blocks/                            # Reusable theme blocks
├── snippets/                          # Shared Liquid snippets
├── layout/
│   └── theme.liquid                   # Main theme layout
├── templates/                         # Page templates (JSON + Liquid)
├── config/
│   ├── settings_schema.json           # Global theme settings schema
│   └── settings_data.json             # Saved theme settings
├── locales/                           # Translation files (20+ languages)
├── .theme-check.yml                   # Linter config (documents all suppressions)
└── README.md
```

---

## 🧩 How It Works — Architecture Flow

### Trust Signal Popup Modal

```
User clicks trust signal link
        │
        ▼
 ┌──────────────────────────────────┐
 │   Is the URL same-origin?        │
 └──────────────────────────────────┘
        │                    │
       YES                   NO
        │                    │
        ▼                    ▼
 fetch(relative path)   <iframe src="URL">
 credentials: include   (direct embed)
        │                    │
        ▼                    ▼
 DOMParser + inject     Site loads if it
 into srcdoc iframe     allows iframing
 (bypasses             ┌──────────────┐
  X-Frame-Options)     │ fallback bar │
        │              │ "Open in new │
        ▼              │  tab ↗"     │
 Full page renders     └──────────────┘
 with original CSS
 inside modal
```

### Bundle Picker

```
Theme Customizer
  bundle_img_fit / bundle_img_ratio
        │
        ▼
 CSS Custom Properties
  --pia-bundle-img-fit
  --pia-bundle-img-ratio
        │
        ▼
 .product-info-advanced__bundle-img
  object-fit: var(--pia-bundle-img-fit)
  aspect-ratio: var(--pia-bundle-img-ratio)
        │
        ▼
 User selects bundle → JS updates
 price, quantity, variant fields
 → Add to Cart form submits
```

### Main Image Badge

```
Schema settings
  badge_position / badge_margin
  badge_font_size / badge_pad_v / badge_pad_h
  badge_icon_size
        │
        ▼
 Liquid calculates
  --pia-badge-top / right / bottom / left
  --pia-badge-font-size
  --pia-badge-icon-size
        │
        ▼
 CSS positions .product-info-advanced__badge
 as absolute overlay on the product image
```

---

## 🚀 Quick Start

1. **Upload theme** — Push to a Shopify store via Shopify CLI:
   ```bash
   shopify theme push --theme <theme-id> --allow-live
   ```

2. **Selective push** (recommended — preserves all saved customizer settings):
   ```bash
   shopify theme push --theme <theme-id> \
     --only sections/product-info-advanced.liquid \
     --only assets/product-info-advanced.css \
     --only assets/product-info-advanced.js \
     --allow-live
   ```

3. **Add the section to your product template** — In the Theme Editor, add `Product Info Advanced` to your product page template and configure settings.

4. **Configure Trust Signals** — For each trust signal block:
   - Set the **link behavior** to `Open Popup / Modal`
   - Set the **link URL** to a Shopify page (`/pages/guarantee`) for best results
   - External URLs are supported but subject to the target site's iframe policy

---

## 🎨 Theme Customizer — Product Info Advanced Options

### Main Image Badge
| Setting | Type | Description |
|---|---|---|
| Badge text | Text | Label displayed on the badge |
| Badge background color | Color | Badge background |
| Badge font color | Color | Text and icon color |
| Font size | Range (10–28px) | Badge text size |
| Vertical padding | Range (4–24px) | Top/bottom padding |
| Horizontal padding | Range (8–32px) | Left/right padding |
| Position | Select | Top-left / Top-right / Bottom-left / Bottom-right |
| Margin from edge | Range (4–40px) | Distance from the corner |
| Badge icon / image | Image | Optional icon shown beside badge text |
| Badge icon size | Range (16–80px) | Size of the custom badge image |

### Bundle Card — Image
| Setting | Type | Description |
|---|---|---|
| Image fit | Select | `cover` / `contain` / `fill` / `auto` |
| Aspect ratio | Select | `1:1 Square` / `4:5 Portrait` / `Auto` |

### Trust Signals
| Setting | Type | Description |
|---|---|---|
| Country flag image | Image | Flag or icon image |
| Flag label text | Text | Text displayed after the flag |
| Link behavior | Select | Redirect / Open Popup / Open New Tab |
| Link URL | URL | Target URL for the link behavior |
| Enable pulse dot | Checkbox | Animated green pulse indicator |

### Media Carousel (Mobile)
| Setting | Description |
|---|---|
| 1 Card | Full-width single card |
| 1.5 Cards | One card + peek of next |
| 2 Cards | Two equal cards |
| **2.5 Cards** | Two cards + peek of next |

---

## 🛒 Compatibility

| Setup | Notes |
|---|---|
| Any Online Store 2.0 theme | Copy `sections/product-info-advanced.liquid`, `assets/product-info-advanced.css`, `assets/product-info-advanced.js` into your theme |
| Shopify pages for modal | `/pages/` URLs load fully in the popup modal with all CSS preserved |
| External URLs for modal | Embedded via iframe — works on sites that allow framing; "Open in new tab" fallback always visible |
| Subscription apps | Subscription toggle is self-contained; compatible with any app that processes the standard Shopify subscription selling plan form fields |
| Bundle apps | Bundle picker is a standalone Liquid + JS implementation; no external app required |

---

## 🧪 Testing Checklist

- [ ] Trust Signal pulse dot aligns correctly with other list items
- [ ] Flag text appears after flag image in each trust signal
- [ ] "Open Popup" modal opens and displays page content
- [ ] Modal close button (×) dismisses the modal
- [ ] Media Carousel shows 2.5 cards on mobile when set
- [ ] Bundle image respects configured fit and aspect ratio
- [ ] Main Image Badge appears in the correct corner with correct styling
- [ ] Badge custom icon resizes with the icon size slider
- [ ] Subscription toggle switches between one-time and subscribe price
- [ ] Add to Cart form submits correctly

---

## 📄 Key Files

| File | Purpose |
|---|---|
| `sections/product-info-advanced.liquid` | Main section: Liquid rendering, CSS custom property injection, full schema |
| `assets/product-info-advanced.css` | All section styles including modal, badge, bundle, trust signals, carousel |
| `assets/product-info-advanced.js` | Interactive logic: modal fetch/render, bundle selection, subscription toggle, carousel |
| `.theme-check.yml` | Linter config: documents `UnclosedHTMLElement` (Liquid conditional false positive) and `CSSLint` (`@starting-style`) suppressions |

---

## 🐛 Troubleshooting

| Issue | What to try |
|---|---|
| Modal shows "refused to connect" | The target site blocks iframes via `X-Frame-Options`. Use a Shopify page (`/pages/…`) instead — these always load correctly |
| Modal shows blank / empty | Check that the URL is a valid, publicly accessible page. Password-protected store pages may not load |
| Badge doesn't appear | Ensure a Badge text or image is set in the customizer and that the section is saved |
| Bundle images look stretched | Set `Image fit` to `contain` and `Aspect ratio` to `1:1 Square` in the Bundle Card settings |
| Pulse dot misaligns trust signal | Ensure you're on the latest version — alignment was fixed in `4c6c5a1` |

---

## 📚 Changelog

See commit history for full change log. Key milestones:

| Commit | Description |
|---|---|
| `4c6c5a1` | Initial feature set: trust signals, badge controls, carousel 2.5, bundle image options |
| `89380b4` | Fixed all 30 theme-check warnings |
| `05e5850` | Popup modal: replaced iframe with fetch + inject; added badge icon size control |
| `23c5b1b` | Robust popup: CORS handling, smart content extraction, `DOMParser` render |
| `243715b` | Popup: switched to `srcdoc` iframe for full CSS-preserving render |
| `5629931` | Popup: correct same-origin vs external URL routing |
| `5dc823d` | Popup external URLs: "Open in new tab" fallback bar |

---

## 📄 License

MIT. Free for personal and commercial use. See LICENSE. Attribution appreciated but not required.

---

💬 **Contact:** rsusano123s@gmail.com · [ecomlanders.co](https://ecomlanders.co)
