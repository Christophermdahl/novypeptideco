# Novy Peptide Co. — Go High Level + WooCommerce Deployment Guide

## What's Included

| File | Purpose |
|------|---------|
| `styles.css` | Shared stylesheet — all design tokens, components, responsive breakpoints |
| `index.html` | Homepage (12+ conversion sections) |
| `shop.html` | Shop page with 20 products, filter chips, sort |
| `product.html` | Single product page template (dynamic via ?id= parameter) |
| `about.html` | About page with mission, standards, process, values |
| `faq.html` | FAQ page (5 categories, 17 questions) |
| `contact.html` | Contact page with GHL-ready form |
| `privacy.html` | Privacy Policy |
| `terms.html` | Terms & Conditions |
| `shipping.html` | Shipping Policy |
| `refund.html` | Refund & Return Policy |
| `IMG_8146.jpeg` | Logo image (used across all pages) |

---

## Go High Level Setup

### Option A: Custom HTML Blocks (Recommended)

GHL allows full custom HTML/CSS/JS blocks inside funnels and websites.

1. **Create a new GHL Website** → Settings → Websites → + New Website
2. **For each page**, add a single "Custom HTML" block
3. **Paste the full HTML** of each page into the block
4. **Host `styles.css` externally** (see below) OR inline the CSS into each page's `<head>` using a `<style>` tag

### Option B: GHL Funnel Pages

If you prefer funnels:
1. Create a funnel with one step per page
2. Use "Custom Code" elements for each section
3. Add the CSS via the funnel's global `<head>` code injection

### Hosting styles.css

GHL doesn't natively host standalone CSS files. Options:

- **CDN hosting**: Upload `styles.css` to a CDN (e.g., Cloudflare R2, AWS S3, or a simple web server) and reference it via `<link>` tag
- **Inline**: Copy the full contents of `styles.css` into a `<style>` block in each page's `<head>` section (increases page size but works without external hosting)
- **GHL Code Injection**: Go to Settings → Websites → Global Code → Head Code, and paste the full CSS in a `<style>` tag — this applies it site-wide

### Images

Upload `IMG_8146.jpeg` (your logo) to GHL Media Library or host on a CDN. Then update all `src="IMG_8146.jpeg"` references to the full URL.

---

## GHL Form / CRM Integration

### Opt-in Forms (Homepage, Shop, About)

Every opt-in form has `data-gohighlevel-form` attributes and collects: `first_name`, `email`, `phone`, `sms_consent`.

To connect to GHL:

1. In GHL, go to **Automation → Workflows**
2. Create a workflow triggered by **Form Submission** or **Webhook**
3. Copy the webhook URL
4. In each HTML file, find the comment:
   ```
   // GHL webhook: fetch('https://services.leadconnectorhq.com/hooks/YOUR_WEBHOOK_ID'...
   ```
5. Uncomment and replace `YOUR_WEBHOOK_ID` with your actual webhook ID
6. The form data (name, email, phone, sms_consent) will POST as JSON

### Contact Form

Same process — the contact form collects `first_name`, `last_name`, `email`, `phone`, `subject`, `message`, and `sms_consent`. Connect to a separate GHL workflow for support ticket routing.

### GHL Automation Suggestions

- **Welcome Sequence**: Trigger when email is captured → Send welcome email → Wait 1 day → Send product education email → Wait 2 days → Send 10% off reminder
- **Abandoned Cart**: If using WooCommerce cart, trigger on cart abandonment → Send 3-email sequence
- **Post-Purchase**: Trigger on order completion → Send order confirmation → Wait 3 days → Send COA/tracking → Wait 7 days → Request review
- **SMS Opt-in**: Separate workflow for SMS subscribers → Send confirmation → Add to SMS broadcast list

---

## WooCommerce Integration

### Product Cards

Every product card has WooCommerce-compatible data attributes:
```html
<div class="product-card" data-product-id="tirz-30" data-cat="weight" data-price="149">
```

### Connecting to WooCommerce

**Option 1: WooCommerce Headless API**
- Use the WooCommerce REST API to pull real product data (prices, stock, images)
- Replace the static product cards with dynamically rendered ones via JavaScript

**Option 2: WooCommerce Embedded Cart**
- Use WooCommerce's "Buy Now" buttons or embeddable cart widgets
- Replace `onclick="addToCart(event, '...')"` with WooCommerce's `?add-to-cart=ID` URL pattern

**Option 3: GHL Native Ecommerce**
- GHL has built-in products, order forms, and payment processing
- Create products in GHL → Settings → Payments → Products
- Replace the "Add to Cart" buttons with GHL order form links or embed GHL checkout forms

### Product IDs to Set Up

| data-product-id | Product Name | Price |
|-----------------|-------------|-------|
| reta-10 | Retatrutide 10mg | $109 |
| reta-20 | Retatrutide 20mg | $189 |
| reta-30 | Retatrutide 30mg | $259 |
| reta-50 | Retatrutide 50mg | $389 |
| tirz-10 | Tirzepatide 10mg | $89 |
| tirz-30 | Tirzepatide 30mg | $149 |
| tirz-60 | Tirzepatide 60mg | $249 |
| sema-30 | Semaglutide 30mg | $129 |
| bpc-10 | BPC-157 10mg | $59 |
| wolv-20 | Wolverine 20mg | $119 |
| tesa-10 | Tesamorelin 10mg | $89 |
| tesa-20 | Tesamorelin 20mg | $149 |
| serm-10 | Sermorelin 10mg | $69 |
| cjc-1295 | CJC-1295 | $79 |
| motc-10 | MOTS-c 10mg | $99 |
| nad-500 | NAD+ 500mg | $119 |
| mt2 | Melanotan 2 | $49 |
| glow | GLOW | $179 |
| klow | KLOW | $229 |
| bac-water | Bacteriostatic Water 30ml | $8 |

---

## Brand Assets Still Needed

Before going live, you'll need:

1. **Product photography** — Real vial photos for each product (replace the CSS vial placeholders)
2. **Hero banner image** — High-res lifestyle/lab photo for the homepage hero
3. **About page imagery** — Lab photos, team photos, or facility shots
4. **Favicon** — 32x32 and 180x180 versions of your logo for browser tabs
5. **Open Graph image** — 1200x630px branded image for social media sharing
6. **SSL certificate** — Ensure novypeptideco.com has HTTPS enabled (GHL handles this)
7. **Google Analytics / Meta Pixel** — Add tracking codes to GHL's global head code injection

---

## Color Reference

| Token | Hex | Usage |
|-------|-----|-------|
| Navy Deep | #050D1F | Footer, age gate overlay, dark sections |
| Blue 600 | #0057FF | Primary CTA, links, accents |
| Blue 500 | #1A7DFF | Hover states |
| Blue 400 | #4A9BFF | Highlights, dark-bg accents |
| Silver 100 | #F4F6FA | Light section backgrounds |
| Silver 200 | #E5E9F0 | Borders |
| Ink | #0A0F1A | Headlines, dark buttons |
| White | #FFFFFF | Primary background |

## Typography

- **Display / Headlines**: Rajdhani (Google Fonts)
- **Body / UI**: Inter (Google Fonts)

---

## Going Live Checklist

- [ ] Upload all files to GHL
- [ ] Host styles.css externally or inline into each page
- [ ] Upload logo to GHL Media Library and update image paths
- [ ] Replace product vial CSS graphics with real product photos
- [ ] Set up GHL webhook endpoints for all forms
- [ ] Create GHL workflows (welcome, contact, post-purchase)
- [ ] Set up WooCommerce or GHL products with correct IDs
- [ ] Add favicon and OG image
- [ ] Add Google Analytics / Meta Pixel tracking
- [ ] Test age gate on fresh browser session
- [ ] Test all navigation links
- [ ] Test mobile responsiveness
- [ ] Test form submissions flow into GHL CRM
- [ ] Review all legal pages with your attorney
- [ ] Enable SSL on novypeptideco.com
- [ ] Launch
