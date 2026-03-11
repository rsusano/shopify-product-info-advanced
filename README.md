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
| **Open Popup / Modal** | Click any trust signal link to load content in a modal — works with same-origin Shopify pages (full fetch + render) and external URLs (direct iframe) |
| **Media Carousel** | Scrollable image/video carousel with configurable visible cards: 1, 1.5, 2, or **2.5** on mobile |
| 📦 **Bundle Options** | App-free product bundle picker — configurable image fit, aspect ratio, pricing, and variant selection. **No bundle app required.** |
| **Main Image Badge** | Fully customizable badge overlay: font size, font color, padding, position (4 corners), margin, and icon size — all via the customizer |
| **Benefit Icons** | Icon + text benefit list with configurable layout |
| 🔄 **Subscription Toggle** | Built-in subscribe & save toggle — dynamically switches prices and selling plans. **No subscription app required for display.** |
| **Quantity & Add to Cart** | Integrated quantity selector and ATC form using `routes.cart_add_url` |

---

## 📦 Bundle Options — Deep Dive

> **Zero app dependencies.** The bundle picker is a fully self-contained Liquid + JS + CSS implementation. No Kaching, No Bundle Bear, no third-party code needed.

### What it does
- Displays multiple product variants as visual "bundle cards" — each with its own image, title, quantity, and price
- Selecting a bundle card updates the ATC form's variant ID, quantity, and displayed price in real time
- Bundle images are fully configurable: choose fit mode and aspect ratio to match your product imagery

### Bundle Image Options

| Setting | Options | Effect |
|---|---|---|
| **Image fit** | `cover` | Fills the card; crops if needed. Best for lifestyle shots |
| | `contain` | Shows the full image with letterboxing. Best for product-on-white |
| | `fill` | Stretches to fill exactly — use with caution |
| | `auto` | Natural image dimensions |
| **Aspect ratio** | `1:1 Square` | Uniform square cards — clean grid look |
| | `4:5 Portrait` | Taller cards — great for supplement bottles |
| | `Auto` | Matches natural image proportions |

### How the Bundle Picker works

```
Theme Customizer
  bundle_img_fit ──────────────────────┐
  bundle_img_ratio ────────────────────┤
                                       ▼
                            CSS Custom Properties injected
                            by Liquid at render time:
                              --pia-bundle-img-fit
                              --pia-bundle-img-ratio
                                       │
                                       ▼
                         .product-info-advanced__bundle-img
                           object-fit:   var(--pia-bundle-img-fit)
                           aspect-ratio: var(--pia-bundle-img-ratio)
                                       │
                    ┌──────────────────┴──────────────────┐
                    │                                     │
                    ▼                                     ▼
           User clicks bundle card             JS updates the form:
                    │                           - variant_id hidden input
                    │                           - quantity input
                    │                           - displayed price text
                    ▼                                     │
           Card gets .is-selected state                   ▼
           (visual highlight ring)           Add to Cart submits
                                             correct bundle to cart
```

---

## 🔄 Subscription Toggle — Deep Dive

> **Built-in subscribe & save — no app required for the toggle logic.** Works alongside any subscription app (Recharge, Skio, Bold, etc.) that reads standard Shopify selling plan form fields.

### What it does
- Renders a one-click toggle between **One-time purchase** and **Subscribe & Save**
- When subscribe mode is active: dynamically swaps the displayed price to the subscription price and injects the correct `selling_plan` field into the ATC form
- When one-time mode is active: removes the `selling_plan` field so the standard one-time price and variant apply
- Price swap is instant — no page reload, no AJAX

### Subscription Toggle flow

```
Product page loads
        │
        ▼
 Liquid renders both prices:
  - One-time price (from variant)
  - Subscribe price (from selling_plan_allocation)
        │
        ▼
 Toggle renders in DEFAULT state
 (configured in customizer: one-time or subscribe)
        │
        ├─────────────────────────────────────────┐
        │                                         │
        ▼                                         ▼
 User clicks "Subscribe & Save"        User clicks "One-time"
        │                                         │
        ▼                                         ▼
 JS sets isSubscribed = true           JS sets isSubscribed = false
        │                                         │
        ▼                                         ▼
 - Toggle gets .is-active class        - Toggle loses .is-active class
 - Price display swaps to              - Price display swaps back to
   subscribe price                       one-time price
 - Hidden <input name="selling_plan">  - selling_plan input REMOVED
   injected into ATC form                from ATC form
        │                                         │
        └──────────────┬──────────────────────────┘
                       │
                       ▼
              Add to Cart submits
              with correct selling_plan
              (or without for one-time)
                       │
                       ▼
           Subscription app (Recharge,
           Skio, Bold, etc.) reads the
           standard selling_plan field
           and processes accordingly
```

### Compatibility with subscription apps

| App | How it works |
|---|---|
| **Recharge** | Reads `selling_plan` from the ATC form — fully compatible |
| **Skio** | Same standard selling plan field — fully compatible |
| **Bold Subscriptions** | Reads Shopify selling plan allocations — fully compatible |
| **Native Shopify Subscriptions** | Built on the same selling plan API — fully compatible |
| **Any other app** | As long as the app processes `selling_plan` from form data — compatible |

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

### 📦 Bundle Card — Image & Display
| Setting | Type | Description |
|---|---|---|
| Bundle title | Text | Label shown above the bundle card |
| Bundle image | Image | Product image for this bundle option |
| **Image fit** | Select | `cover` / `contain` / `fill` / `auto` — controls how the image fills its container |
| **Aspect ratio** | Select | `1:1 Square` / `4:5 Portrait` / `Auto` — sets card proportions |
| Bundle variant | Variant picker | The product variant this bundle card selects |
| Bundle quantity | Number | How many units this bundle adds to cart |
| Bundle price label | Text | Optional price or saving label (e.g. "Save 20%") |

### 🔄 Subscription Toggle
| Setting | Type | Description |
|---|---|---|
| Show subscription toggle | Checkbox | Enable/disable the subscribe & save toggle |
| Default state | Select | Start on `One-time` or `Subscribe & Save` |
| Subscribe label | Text | Label for the subscribe option (e.g. "Subscribe & Save") |
| One-time label | Text | Label for the one-time option (e.g. "One-time Purchase") |
| Saving badge text | Text | Badge shown on subscribe option (e.g. "Save 15%") |
| Selling plan | Selling plan | The Shopify selling plan to inject when subscribe is active |

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
| 📦 **Bundle — no app needed** | Bundle picker is a fully self-contained Liquid + JS implementation. Replaces Kaching, Bundle Bear, and similar apps for standard bundle UX |
| 🔄 **Subscription — any app** | Toggle injects/removes the standard Shopify `selling_plan` field. Works with Recharge, Skio, Bold, native Shopify Subscriptions, or any app that reads selling plan form data |

---

## 🧪 Testing Checklist

**Trust Signals & Modal**
- [ ] Trust Signal pulse dot aligns correctly with other list items
- [ ] Flag text appears after flag image in each trust signal
- [ ] "Open Popup" modal opens and displays page content
- [ ] Modal close button (×) dismisses the modal

**Media & Badge**
- [ ] Media Carousel shows 2.5 cards on mobile when set
- [ ] Main Image Badge appears in the correct corner with correct styling
- [ ] Badge custom icon resizes with the icon size slider

**📦 Bundle**
- [ ] Bundle cards display with correct image, title, and price label
- [ ] Selecting a bundle card updates the displayed price instantly
- [ ] The correct variant ID and quantity are passed to the ATC form
- [ ] Bundle image respects the configured fit and aspect ratio settings
- [ ] Add to Cart submits the selected bundle variant correctly

**🔄 Subscription**
- [ ] Subscription toggle renders in the configured default state
- [ ] Clicking "Subscribe & Save" swaps the price to the subscription price
- [ ] Clicking "One-time" swaps the price back to the one-time price
- [ ] The `selling_plan` field is present in the form when subscribe is active
- [ ] The `selling_plan` field is absent from the form when one-time is active
- [ ] Add to Cart submits the subscription correctly to the store

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
