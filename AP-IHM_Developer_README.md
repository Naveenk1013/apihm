# AP-IHM Landing Page — Developer Handoff README

**Project:** Asia Pacific Institute of Hotel Management (AP-IHM) — Marketing Landing Page
**File:** `AP-IHM_Landing_Page.html`
**Version:** 1.0.0
**Last Updated:** March 2025
**Prepared by:** Design & Frontend Team

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Tech Stack](#2-tech-stack)
3. [File Structure](#3-file-structure)
4. [Design System — CSS Variables](#4-design-system--css-variables)
5. [Typography](#5-typography)
6. [Page Sections — Map & IDs](#6-page-sections--map--ids)
7. [Frontend Components Guide](#7-frontend-components-guide)
8. [Backend Integration Points](#8-backend-integration-points)
9. [WhatsApp CTA Integration](#9-whatsapp-cta-integration)
10. [Images — Lazy Loading System](#10-images--lazy-loading-system)
11. [JavaScript Functions Reference](#11-javascript-functions-reference)
12. [Responsive Breakpoints](#12-responsive-breakpoints)
13. [Performance Notes](#13-performance-notes)
14. [Known Placeholders & TODOs](#14-known-placeholders--todos)
15. [Recommended Next Steps](#15-recommended-next-steps)

---

## 1. Project Overview

This is a **single-file HTML landing page** for AP-IHM's 2025–26 admissions cycle. It is intentionally self-contained — all CSS and JavaScript are embedded inline within the `<style>` and `<script>` tags respectively. There are **no external JS dependencies** (no jQuery, no frameworks). The only external resources loaded are Google Fonts.

The page serves two primary conversion goals:

- **Primary CTA:** Admissions enquiry form (hero section, top of page)
- **Secondary CTA:** WhatsApp conversation initiation (FAB button + CTA section)

---

## 2. Tech Stack

| Layer        | Technology                                      |
|--------------|-------------------------------------------------|
| Markup       | Semantic HTML5                                  |
| Styling      | CSS3 with custom properties (CSS variables)     |
| Scripting    | Vanilla JavaScript (ES6+), no dependencies      |
| Fonts        | Google Fonts — Cormorant Garamond + DM Sans     |
| Images       | Unsplash CDN (placeholder) — **replace in prod**|
| Animation    | CSS `@keyframes` + IntersectionObserver API     |
| Lazy Loading | Native IntersectionObserver — no library needed |

> **For future builds:** If the team migrates to a component framework (React, Vue, Next.js), all CSS variables and the design system defined in `:root` can be directly ported to a global stylesheet or Tailwind config without any visual changes.

---

## 3. File Structure

Currently the entire project lives in one file. When splitting for a proper build system, the recommended structure is:

```
apihm-website/
├── index.html
├── assets/
│   ├── css/
│   │   ├── variables.css       ← Extract :root block here
│   │   ├── base.css            ← Reset, body, utility classes
│   │   ├── nav.css
│   │   ├── hero.css
│   │   ├── sections.css        ← Why, Programs, Curriculum, etc.
│   │   ├── careers.css
│   │   ├── faculty.css
│   │   ├── testimonials.css
│   │   ├── footer.css
│   │   └── animations.css      ← @keyframes, .reveal classes
│   ├── js/
│   │   ├── nav.js              ← Scroll + hamburger logic
│   │   ├── lazyLoad.js         ← IntersectionObserver for images
│   │   ├── revealAnimations.js ← Scroll reveal observer
│   │   ├── accordion.js        ← toggleAcc()
│   │   └── form.js             ← handleApply() + validation + API call
│   └── images/
│       └── (downloaded & optimised images go here)
├── README.md
└── .env                        ← API keys, WhatsApp number, endpoint URLs
```

---

## 4. Design System — CSS Variables

All colours, spacing, shadows, and transitions are defined in `:root` at the very top of the `<style>` block. **Never hardcode colour hex values anywhere in the file.** Always reference a variable.

```css
:root {
  /* ── Brand Colours ── */
  --burgundy:      #6B1530;   /* Primary brand colour */
  --burgundy-deep: #4A0E20;   /* Darker variant — hover states, gradients */
  --burgundy-mid:  #8C1E3C;   /* Mid variant — faculty avatars */
  --gold:          #C9941A;   /* Accent — labels, dots, icons */
  --gold-light:    #E8B94A;   /* Lighter gold — hero headings, stat numbers */
  --gold-pale:     #F5E6C0;   /* Very light gold — pill backgrounds, tints */

  /* ── Neutrals ── */
  --cream:         #FDF6EE;   /* Page background */
  --ivory:         #FAF3E8;   /* Alternate section background */
  --dark:          #1A0810;   /* Near-black — headings, dark sections */
  --charcoal:      #2C1A24;   /* Unused currently — available for cards */
  --text:          #3D2030;   /* Default body text */
  --text-muted:    #7A5A68;   /* Subtitles, captions, descriptions */
  --white:         #FFFFFF;

  /* ── Effects ── */
  --shadow-card:   0 4px 32px rgba(107,21,48,0.10);
  --shadow-hover:  0 12px 48px rgba(107,21,48,0.22);

  /* ── Shape ── */
  --radius-sm:     8px;
  --radius-md:     16px;
  --radius-lg:     28px;

  /* ── Motion ── */
  --transition:    0.38s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
```

---

## 5. Typography

Two Google Font families are used. Load order matters — do not reorder the `<link>` tags.

| Role               | Family                  | Weight(s)           | Usage                                      |
|--------------------|-------------------------|---------------------|--------------------------------------------|
| Display / Headings | `Cormorant Garamond`    | 300, 400, 600, 700  | All `<h1>–<h4>`, section titles, numbers   |
| Body / UI          | `DM Sans`               | 300, 400, 500, 600  | Paragraphs, labels, buttons, nav, forms    |

**Rule:** All `section-title` class elements use `font-family: 'Cormorant Garamond', serif`. All form elements, nav links, and body copy use `'DM Sans', sans-serif`.

```css
/* Display heading example */
.section-title {
  font-family: 'Cormorant Garamond', serif;
  font-weight: 600;
  line-height: 1.1;
  color: var(--dark);
}
```

---

## 6. Page Sections — Map & IDs

| Section ID         | Purpose                                  | Background Colour     |
|--------------------|------------------------------------------|-----------------------|
| `#main-nav`        | Fixed navigation bar                     | Transparent → cream   |
| `#hero`            | Hero + Admissions form                   | Dark image overlay    |
| `.stats-strip`     | Animated ticker bar (no ID)              | `var(--burgundy)`     |
| `#why`             | Why choose AP-IHM                        | `var(--white)`        |
| `#programs`        | BHMCT, BBAHM, Certification cards        | `var(--cream)`        |
| `#curriculum`      | 4-year accordion + subject pills         | `var(--white)`        |
| `#careers`         | Career opportunity cards                 | `var(--dark)`         |
| `#infrastructure`  | Campus facilities cards                  | `var(--cream)`        |
| `#faculty`         | Faculty & expert cards                   | `var(--white)`        |
| `#testimonials`    | Auto-scrolling testimonial carousel      | `var(--ivory)`        |
| `#cta-section`     | Final CTA with Apply + WhatsApp buttons  | Burgundy gradient     |
| `footer`           | Links, contact, accreditation            | `var(--dark)`         |
| `.wa-fab`          | Floating WhatsApp button                 | `#25D366` (WhatsApp)  |

---

## 7. Frontend Components Guide

### 7.1 Navigation

- Fixed at top, transparent initially, turns frosted glass on scroll (`nav.scrolled` class toggled at 60px scroll offset).
- Mobile: hamburger reveals `.nav-links` as a fullscreen overlay via `.open` class.
- **To add new nav items:** Add `<li><a href="#sectionId">Label</a></li>` inside `<ul id="nav-links">`.

### 7.2 Hero Section

- CSS grid: `1fr 420px` on desktop, stacks on mobile.
- Background image uses the lazy load system — change the `data-src` attribute on `.hero-bg-img` to update.
- Hero entry animations are CSS `@keyframes` with `animation-delay` — they fire immediately on page load (not scroll-triggered). Modify delay values in `.hero-eyebrow`, `.hero-title`, `.hero-subtitle`, `.hero-badges`, `.hero-stats`, `.hero-form-card`.

### 7.3 Admissions Form

```html
<!-- Located inside #hero, class: .hero-form-card -->
```

Fields currently present:
- First Name, Last Name (grid row)
- Email Address
- Mobile Number
- Program (select dropdown — 4 options)
- City / State

The submit button calls `handleApply(event)` in JS. **This function currently only shows a visual confirmation — it does NOT send data anywhere.** See [Backend Integration Points](#8-backend-integration-points) for wiring instructions.

**To add a new field:** Copy any `.form-group` block and add it before the `<button class="btn-submit">`.

**To add a program option:**
```html
<select>
  ...
  <option>NEW PROGRAM NAME HERE</option>
</select>
```

### 7.4 Stats Ticker

Infinite horizontal scroll animation via CSS `@keyframes ticker`. The inner list is **duplicated** to create a seamless loop. If you add or remove items, duplicate the entire list again so the loop stays seamless.

### 7.5 Accordion (Curriculum Section)

Controlled by `toggleAcc(header)` in JavaScript. Opens one item at a time (closes others). Item state is managed with the `.open` class on `.acc-item`. Max-height of `.acc-body` transitions from `0` to `400px` when open — **if adding many subjects, increase `max-height: 400px` in `.acc-item.open .acc-body`**.

### 7.6 Testimonials Carousel

Auto-scrolls using CSS `@keyframes scrollTestis`. The card list is duplicated for a seamless loop (same technique as the ticker). Animation pauses on mouse hover. **To add a testimonial:** Add a `.testi-card` block in both the original and duplicate halves of `#testi-track`.

### 7.7 Faculty Cards

Each `.faculty-card` uses a two-letter text avatar (initials). When real photos are available, replace the `.faculty-avatar` div with an `<img>` tag styled to `width:60px; height:60px; border-radius:50%; object-fit:cover;`.

### 7.8 WhatsApp FAB

Floating action button fixed at `bottom: 2rem; right: 2rem`. On mobile screens below 520px, the label text is hidden (only icon shows) via `display: none` on `.wa-fab span`. See [WhatsApp CTA Integration](#9-whatsapp-cta-integration) for the URL format.

---

## 8. Backend Integration Points

### 8.1 Admissions Form Submission

**Current behaviour:** The `handleApply()` function shows a success message and resets after 3.5 seconds. No data is transmitted.

**What needs to be built:**

```javascript
// REPLACE the current handleApply() function with this pattern:
async function handleApply(e) {
  e.preventDefault();
  const btn = e.target;

  // 1. Collect form data
  const payload = {
    firstName:  document.querySelector('input[placeholder="Rahul"]').value,
    lastName:   document.querySelector('input[placeholder="Shah"]').value,
    email:      document.querySelector('input[type="email"]').value,
    mobile:     document.querySelector('input[type="tel"]').value,
    program:    document.querySelector('select').value,
    city:       document.querySelector('input[placeholder="Ahmedabad, Gujarat"]').value,
    source:     'landing_page',
    timestamp:  new Date().toISOString(),
  };

  // 2. Basic validation (add more as needed)
  if (!payload.email || !payload.mobile || !payload.program) {
    alert('Please fill all required fields.');
    return;
  }

  // 3. Show loading state
  btn.textContent = 'Submitting…';
  btn.disabled = true;

  // 4. POST to your API endpoint
  try {
    const response = await fetch('YOUR_API_ENDPOINT_HERE', {
      method:  'POST',
      headers: { 'Content-Type': 'application/json' },
      body:    JSON.stringify(payload),
    });

    if (!response.ok) throw new Error('Server error');

    // 5. Success
    btn.textContent = '✓ Application Received!';
    btn.style.background = 'linear-gradient(135deg, #2E7D32, #1B5E20)';

    // Optional: fire a Google Analytics / Meta Pixel event here
    // gtag('event', 'form_submit', { event_category: 'admissions' });

  } catch (err) {
    btn.textContent = 'Something went wrong. Try again.';
    btn.style.background = 'linear-gradient(135deg, #B71C1C, #7F0000)';
    console.error('Form submission error:', err);
  } finally {
    btn.disabled = false;
    setTimeout(() => {
      btn.textContent = 'Submit Application →';
      btn.style.background = '';
    }, 4000);
  }
}
```

**Recommended API endpoint response format:**
```json
{
  "success": true,
  "leadId": "AP-IHM-2025-001234",
  "message": "Application received successfully."
}
```

**Suggested backend integrations:**
- **CRM:** Zoho CRM, Salesforce, or a custom leads table in your DB
- **Email notification:** Trigger a confirmation email to the student via SendGrid / AWS SES
- **WhatsApp notification:** Use WhatsApp Business API to auto-message the lead
- **Spreadsheet logging:** Google Sheets API as a lightweight lead tracker

### 8.2 Field Name Mapping

Currently fields are selected by placeholder text — this is fragile. **Add `name` attributes to all inputs before backend integration:**

```html
<!-- Before -->
<input type="text" placeholder="Rahul" />

<!-- After -->
<input type="text" name="firstName" id="firstName" placeholder="Rahul" required />
```

Recommended `name` attributes:

| Field           | Recommended `name`   | Type     |
|-----------------|----------------------|----------|
| First Name      | `firstName`          | text     |
| Last Name       | `lastName`           | text     |
| Email           | `email`              | email    |
| Mobile          | `mobile`             | tel      |
| Program         | `program`            | select   |
| City / State    | `city`               | text     |

### 8.3 Tracking & Analytics

Add these scripts in the `<head>` before going live:

```html
<!-- Google Analytics 4 -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){ dataLayer.push(arguments); }
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>

<!-- Meta Pixel (Facebook Ads) -->
<script>
  !function(f,b,e,v,n,t,s){ /* Meta Pixel base code */ }
  fbq('init', 'YOUR_PIXEL_ID');
  fbq('track', 'PageView');
</script>
```

Fire a conversion event on successful form submission:
```javascript
gtag('event', 'generate_lead', { currency: 'INR', value: 1 });
fbq('track', 'Lead');
```

---

## 9. WhatsApp CTA Integration

### Current Format

WhatsApp links use the universal `wa.me` URL scheme:

```
https://wa.me/919876543210?text=Hi%2C%20I%27d%20like%20to%20know%20more%20about%20admissions%20at%20AP-IHM%20Ahmedabad.
```

**Format breakdown:**
```
https://wa.me/[COUNTRY_CODE][MOBILE_NUMBER]?text=[URL_ENCODED_PRE_FILLED_MESSAGE]
```

### Instances in the File

There are **4 WhatsApp links** in the file. Update all four when the number changes:

| Location                 | Element                         |
|--------------------------|---------------------------------|
| Hero CTA section         | `.cta-buttons` second button    |
| Footer contact column    | `.footer-links` fourth link     |
| Floating Action Button   | `.wa-fab` `href` attribute      |
| CTA section outline btn  | `#cta-section .btn-outline`     |

**Tip:** Do a find-and-replace on `wa.me/91` to update all instances at once. Replace `9876543210` with the real business number.

### WhatsApp Business API (Recommended for Production)

For automated responses and lead tracking, integrate [WhatsApp Business API](https://business.whatsapp.com/products/business-api) via providers like:
- **Interakt** (popular in India, affordable)
- **Wati.io**
- **AiSensy**

These let you trigger automated welcome messages when a lead clicks the WhatsApp button, which significantly improves response rates.

---

## 10. Images — Lazy Loading System

### How It Works

All images use a custom lazy loading implementation via the `IntersectionObserver` API. Images load **200px before they enter the viewport** (`rootMargin: '200px 0px'`).

**Pattern used:**
```html
<img
  data-src="ACTUAL_IMAGE_URL"
  src="data:image/svg+xml,..."    ← Tiny inline SVG placeholder (no network request)
  alt="Description"
  loading="lazy"
  class=""
/>
```

When the image enters the viewport, the observer swaps `data-src` into `src`, then adds class `.loaded` (which transitions `opacity` from `0` to `1`).

### Replacing Placeholder Images

All images currently point to **Unsplash CDN**. Replace these in production with actual AP-IHM campus photos. To swap an image:

1. Find the `<img>` tag by its `alt` text
2. Replace the `data-src` URL with your hosted image URL
3. Update the placeholder SVG background colour in `src` to roughly match your image (optional — it shows only for a split second)

**Image dimensions used across the page:**

| Section            | Recommended Dimensions | Notes                                 |
|--------------------|------------------------|---------------------------------------|
| Hero background    | 1800 × 1200px          | Full bleed, heavily filtered          |
| Why AP-IHM main    | 900 × 480px            | Portrait-ish, left column             |
| Why AP-IHM accent  | 400 × 400px            | Square, bottom-right floating card    |
| Program cards      | 700 × 200px            | Wide banner, top of card              |
| Infrastructure     | 600 × 180px            | Wide banner, top of card              |
| Curriculum bottom  | 600 × 240px            | Decorative, left column               |

**Recommended image hosting:** Cloudinary (free tier), AWS S3 + CloudFront, or your own server with Nginx serving compressed WebP.

---

## 11. JavaScript Functions Reference

| Function            | Location        | Purpose                                                           |
|---------------------|-----------------|-------------------------------------------------------------------|
| `toggleAcc(header)` | inline `<script>` | Opens/closes accordion items. Called `onclick` on `.acc-header`. |
| `handleApply(e)`    | inline `<script>` | Form submit handler. **Replace with real API call.**             |
| `imgObserver`       | inline `<script>` | IntersectionObserver for lazy-loading images.                    |
| `revealObserver`    | inline `<script>` | IntersectionObserver for scroll-reveal animations.               |
| Nav scroll handler  | inline `<script>` | Toggles `.scrolled` class on `#main-nav` past 60px scroll.      |
| Hamburger handler   | inline `<script>` | Toggles `.open` class on `#nav-links`.                           |
| Ticker hover pause  | inline `<script>` | Pauses stats ticker animation on hover.                          |

### Scroll Reveal System

Three CSS classes trigger different reveal directions:

```css
.reveal       /* Fades in from below (translateY 36px → 0) */
.reveal-left  /* Slides in from left (translateX -40px → 0) */
.reveal-right /* Slides in from right (translateX 40px → 0) */
```

Add `data-delay="1"` through `data-delay="6"` to stagger children:

```html
<div class="reveal" data-delay="2">Content appears 0.20s after trigger</div>
```

The `revealObserver` adds class `.visible` when the element enters the viewport, which triggers the CSS transition.

---

## 12. Responsive Breakpoints

| Breakpoint   | Width       | Changes Applied                                                   |
|--------------|-------------|-------------------------------------------------------------------|
| Desktop      | > 1100px    | Full layout — 2-col hero, 3-col programs, 4-col careers, etc.   |
| Tablet       | ≤ 1100px    | Hero stacks, 2-col programs, 2-col careers, 2-col faculty        |
| Mobile       | ≤ 768px     | Hamburger nav, all grids collapse to 1-col, why-img-accent hides |
| Small Mobile | ≤ 520px     | 1-col careers/infra, form row stacks, WhatsApp FAB icon-only     |

Media queries are all located at the **bottom of the `<style>` block**, clearly labelled `/* ===== RESPONSIVE =====  */`.

---

## 13. Performance Notes

- **No JavaScript framework or library** — page load is extremely lean.
- **Google Fonts** is the only render-blocking external resource. It's preconnected via `<link rel="preconnect">` to minimise impact.
- **All images are lazy-loaded** — only the hero background loads above the fold (and it's low-priority as it's decorative).
- **CSS animations** are GPU-accelerated (using `transform` and `opacity` only — no `top`, `left`, `width` transitions).
- **Ticker and carousel** use CSS `@keyframes` — zero JS runtime cost.

**Before production, run through:**
- [ ] Google PageSpeed Insights
- [ ] Replace all Unsplash URLs with self-hosted WebP images
- [ ] Add `width` and `height` attributes to all `<img>` tags to prevent CLS
- [ ] Minify the HTML file (use html-minifier or your build pipeline)
- [ ] Add `<meta>` OG tags for WhatsApp / Facebook / LinkedIn previews

---

## 14. Known Placeholders & TODOs

These are items that need real data before the page goes live:

| Item                       | Current Value                     | Action Required                              |
|----------------------------|-----------------------------------|----------------------------------------------|
| WhatsApp Number            | `9876543210`                      | Replace with real AP-IHM WhatsApp number     |
| Email in footer            | `info@apihmahmedabad.edu.in`      | Confirm real email address                   |
| Phone in footer            | `+91 98765 43210`                 | Replace with real contact number             |
| Address                    | "Ahmedabad, Gujarat"              | Add full street address                      |
| Social media links         | All `href="#"`                    | Add real Facebook, Instagram, YouTube, LinkedIn URLs |
| Hero background image      | Unsplash hotel pool photo         | Replace with actual campus or hospitality photo |
| All section images         | Unsplash CDN URLs                 | Replace with AP-IHM campus photos            |
| Form API endpoint          | Not connected                     | Wire `handleApply()` to real backend         |
| Alumni stats               | 500+, 30+, 15+                    | Confirm with management, update if different |
| Testimonials               | Fictional names & quotes          | Replace with real student testimonials       |
| Faculty photos             | Text initials avatars             | Replace with real faculty headshots          |
| GTU affiliation year       | Not mentioned                     | Add year of affiliation if required          |
| Google Analytics ID        | Not included                      | Add GA4 tracking tag before launch           |

---

## 15. Recommended Next Steps

### Immediate (Before Go-Live)
1. **Replace all placeholder content** — images, phone numbers, email, address, testimonials
2. **Wire the admissions form** to your CRM / database endpoint
3. **Add `name` and `id` attributes** to all form inputs
4. **Update WhatsApp number** across all 4 instances
5. **Add Google Analytics 4** and Meta Pixel tracking
6. **Add social media links** in the footer
7. **Test on real mobile devices** (iOS Safari, Android Chrome)

### Short Term (Sprint 1–2)
- Add a **Thank You page** or modal after successful form submission
- Implement **form field validation** (regex for phone/email, required fields)
- Add **reCAPTCHA v3** to prevent spam leads
- Create a **leads dashboard** (admin panel) to view and manage submitted applications
- Set up **automated email confirmation** to the student on form submission

### Medium Term (Sprint 3–5)
- Migrate to a **proper build system** (Vite + SCSS) using the file structure in [Section 3](#3-file-structure)
- Add a **Blog / News section** for hospitality industry content (SEO)
- Build a **separate Program Detail page** for BHMCT with full semester-wise syllabus
- Integrate **Google Reviews** or a review API into the testimonials section
- Add an **interactive virtual campus tour** (YouTube embed or 360° viewer)
- Implement **multi-language support** (English + Gujarati) using `i18n`

### SEO Essentials
```html
<!-- Add these to <head> before launch -->
<meta name="description" content="AP-IHM Ahmedabad — AICTE approved hotel management institute offering BHMCT, BBAHM and certification courses. GTU affiliated. Apply for 2025–26 admissions.">
<meta name="keywords" content="hotel management ahmedabad, BHMCT Gujarat, AP-IHM, hospitality courses Gujarat">
<meta property="og:title" content="AP-IHM | Asia Pacific Institute of Hotel Management, Ahmedabad">
<meta property="og:description" content="Shape your hospitality career with AICTE approved programs at AP-IHM Ahmedabad.">
<meta property="og:image" content="URL_TO_YOUR_OG_IMAGE_1200x630.jpg">
<meta property="og:url" content="https://www.yourdomain.com">
<meta name="twitter:card" content="summary_large_image">
<link rel="canonical" href="https://www.yourdomain.com">
```

---

## Contact & Handoff

For questions about design decisions, component logic, or asset requirements, refer back to the original design brief or reach out to the frontend team. All design tokens are centrally managed in `:root` — changing a colour or spacing value there will cascade through the entire page automatically.

**Good luck with the build. 🏨**

---

*This README is specific to version 1.0.0 of `AP-IHM_Landing_Page.html`. Update this document as the codebase evolves.*
