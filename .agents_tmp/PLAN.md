# 1. OBJECTIVE
Reduce page load time by eliminating render‑blocking requests that currently delay the Largest Contentful Paint (LCP) by ~24 seconds. The goal is to bring the page load under 3–5 seconds by self‑hosting critical resources, removing unnecessary dependencies, and applying standard performance optimizations.

# 2. CONTEXT SUMMARY
The site is a single‑page static HTML file (`index.html`) served via GitHub Pages. It currently loads the following render‑blocking external resources in the `<head>`:

1. **Google Fonts** (two requests) – `Inter`, `Space Grotesk`, and `Material Symbols Outlined` from `fonts.googleapis.com`
2. **Tailwind CSS** – full utility library from `cdn.tailwindcss.com` (124 KiB)
3. **Font Awesome** – CSS and icon fonts from `cdnjs.cloudflare.com`
4. **GSAP & ScrollTrigger** – animation libraries from `cdnjs.cloudflare.com`
5. **Polyfill.io** – legacy browser polyfills from `polyfill.io`

All these resources are loaded synchronously, blocking the page render until each finishes downloading. The cumulative delay shown in the performance report is ~24 seconds, with the largest contributors being Tailwind CSS (770 ms), Google Fonts (1,500 ms), and Cloudflare CDN resources (4,310 ms).

The site already inlines critical CSS (custom properties, base styles, component classes) and places JavaScript at the bottom of the body. GSAP‑based animations are currently disabled via comments, but the libraries are still fetched.

# 3. APPROACH OVERVIEW
We will eliminate all render‑blocking external requests by self‑hosting the necessary assets and removing unnecessary dependencies. The strategy includes:

1. **Self‑host fonts** – Download WOFF2 files for Inter, Space Grotesk, and Material Symbols Outlined; serve them from the repository with `font‑display: swap`.
2. **Self‑host Tailwind CSS** – Download the same minified Tailwind CSS from the CDN and serve it locally, removing the external CDN call.
3. **Self‑host Font Awesome** – Download the Font Awesome CSS and webfonts, serve them from a local directory.
4. **Remove polyfill.io** – The site already includes minimal polyfills for older browsers; the external polyfill is redundant and slow.
5. **Remove GSAP & ScrollTrigger** – Since scroll animations are disabled, we can delete the script tags. If needed later, they can be re‑added with `defer`.
6. **Apply `async`/`defer`** – For any remaining external scripts (e.g., FormSubmit.co is already loaded via `fetch`), ensure they are non‑blocking.
7. **Optimize delivery** – Keep the existing inlined critical CSS, add `preconnect` hints for remaining third‑party domains (WhatsApp, Telegram, FormSubmit).

This approach trades a slight increase in repository size for dramatic improvements in load time, reliability, and independence from external CDN availability.

# 4. IMPLEMENTATION STEPS

## Step 1 – Remove polyfill.io
- **Goal:** Eliminate the slow, unnecessary polyfill request.
- **Method:** Delete line 604 (`<script src="https://polyfill.io/v3/polyfill.min.js?features=..."></script>`) from `index.html`.
- **Reference:** `index.html` line 604.

## Step 2 – Analyze used font weights, characters, and icons
- **Goal:** Identify exactly which font weights, styles, characters, and icons are needed to minimize asset sizes.
- **Method:**
  1. Scan `index.html` for Tailwind font‑weight utility classes (`font‑bold`, `font‑medium`, `font‑semibold`, `font‑normal`) and CSS `font‑weight` declarations.
  2. Determine used font weights:
     - **Inter:** 400 (body), 500 (medium), 600 (semibold), 700 (bold) – italic not used.
     - **Space Grotesk:** 700 (headings) – other weights (300‑600) not used.
     - **Material Symbols Outlined:** Only icons "sunny" and "dark_mode".
  3. Character set: Georgian alphabet (Unicode range U+10A0–U+10FF) plus basic Latin.
  4. Extract all Font Awesome icon classes (`fas fa‑*`, `fab fa‑*`) used in the page.
  5. Create a list of required icons (e.g., `globe`, `video`, `chart‑line`, `pen‑nib`, `robot`, `rocket`, `server`, `cogs`, `shopping‑cart`, `home`, `briefcase`, `star`, `check`, `arrow‑right`, `paper‑plane`, `chevron‑down`, `map‑marker‑alt`, `phone`, `envelope`, `whatsapp`, `telegram`, `facebook‑messenger`).
- **Reference:** `index.html` CSS, class names, and icon markup.

## Step 3 – Download and self‑host optimized Google Fonts
- **Goal:** Replace the two Google Fonts `link` tags with subsetted local WOFF2 files under 100 KB each.
- **Method:**
  1. Use [google‑webfonts‑helper](https://gwfh.mranftl.com/fonts) to download subsetted WOFF2 files:
     - **Inter:** Latin + Georgian subset, weights 400,500,600,700, regular only.
     - **Space Grotesk:** Latin + Georgian subset, weight 700 only.
     - **Material Symbols Outlined:** Subset to only the two needed icons (sunny, dark_mode) using a variable‑font subsetting tool (e.g., `pyftsubset` from `fonttools`).
  2. Create a directory `/fonts/` and place the downloaded files there.
  3. Replace the existing `<link rel="preconnect">` and `<link href="https://fonts.googleapis.com/...">` tags with `@font‑face` rules inside the existing `<style>` block.
  4. Add `font‑display: swap;` to each `@font‑face` declaration.
  5. Verify each font file size is under 100 KB (Material Symbols may be slightly larger but still under 200 KB).
- **Reference:** `index.html` lines 58‑61.

## Step 4 – Download and self‑host subsetted Font Awesome
- **Goal:** Remove the external Font Awesome request and serve only the icons actually used.
- **Method:**
  1. Download Font Awesome 6.5.0 free version (CSS + webfonts).
  2. Using the icon list from Step 2, create a custom CSS file that includes only those icons (manually or with a tool like `fontawesome‑subset`).
  3. Subset the WOFF2 font files (`fa‑solid‑900.woff2`, `fa‑brands‑400.woff2`) to contain only the required glyphs using `pyftsubset` (from `fonttools`).
  4. Place the custom CSS in `/fontawesome/css/custom.min.css` and the subsetted webfonts in `/fontawesome/webfonts/`.
  5. Update the `<link>` tag (line 62) to point to the local subsetted CSS: `<link rel="stylesheet" href="/fontawesome/css/custom.min.css">`.
- **Reference:** `index.html` line 62.

## Step 5 – Self‑host Tailwind CSS
- **Goal:** Eliminate the render‑blocking Tailwind CDN request while keeping the same utility classes.
- **Method:**
  1. Download the exact same Tailwind CSS file from `https://cdn.tailwindcss.com/3.4.17`.
  2. Save it as `/css/tailwind.min.css`.
  3. Replace the `<script src="https://cdn.tailwindcss.com"></script>` tag (line 51) with a `<link rel="stylesheet" href="/css/tailwind.min.css">`.
  4. Move the inline `tailwind.config` script (lines 580‑601) after the local CSS link, keeping it intact (it will still work with the local CSS).
- **Reference:** `index.html` lines 51 and 580‑601.

## Step 6 – Remove GSAP & ScrollTrigger scripts
- **Goal:** Drop two unnecessary animation library requests (animations are currently disabled).
- **Method:** Delete lines 54‑55 (`<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/..."></script>`).
- **Reference:** `index.html` lines 54‑55.

## Step 7 – Update remaining external resource hints
- **Goal:** Ensure `preconnect` hints are present for domains that are still used (WhatsApp, Telegram, FormSubmit.co).
- **Method:** Add `<link rel="preconnect" href="https://formsubmit.co">` and consider adding `dns‑prefetch` for other third‑party domains.
- **Reference:** `index.html` head section.

## Step 8 – Verify and test locally
- **Goal:** Confirm that all resources load correctly and the page renders without errors.
- **Method:**
  1. Open `index.html` in a local browser with developer tools.
  2. Check the Network tab for 404s and ensure fonts/icons display.
  3. Test dark‑mode toggle, form submission, and smooth‑scrolling links.
- **Reference:** Entire page.

## Step 9 – Commit and deploy
- **Goal:** Push the optimized version to GitHub Pages.
- **Method:** Commit the new files (`/fonts/`, `/fontawesome/`, `/css/`) and the updated `index.html` to the repository.
- **Reference:** Git workflow.

# 5. TESTING AND VALIDATION

## Expected Outcomes
- **Render‑blocking requests eliminated:** The Lighthouse “Render Blocking Resources” audit should show zero (or minimal) entries.
- **Load time reduced:** Largest Contentful Paint (LCP) should drop from ~24 seconds to under 3–5 seconds in a typical network condition.
- **Font file sizes reduced:** Each font file under 100 KB (Material Symbols under 200 KB), total font payload under 500 KB (down from 3.99 MB).
- **Visual consistency:** All fonts (Inter, Space Grotesk, Material Symbols) and Font Awesome icons display correctly in both light and dark modes.
- **Functionality preserved:** Dark‑mode toggle, smooth scrolling, contact form submission, and service‑button links work without errors.
- **No JavaScript errors:** Console logs should show no missing resources or undefined variables (especially after removing GSAP).

## Validation Steps
1. **Run Lighthouse locally** (Chrome DevTools) or via PageSpeed Insights before and after the changes, comparing the “Performance” score and the “Render‑Blocking Resources” section.
2. **Manual visual inspection:** Verify that all text uses the correct fonts and that Material Symbols icons (sunny/dark_mode) appear.
3. **Font size verification:** In the Network tab, confirm each font file is under 100 KB (Material Symbols under 200 KB) and total font transfer is under 500 KB.
4. **Network tab check:** Load the page with throttling (Fast 3G) and confirm that all resources are served from the same origin (no external CDN calls except for WhatsApp/Telegram/Messenger links).
5. **Cross‑browser quick test:** Open the page in Chrome, Firefox, and Safari to ensure no layout or font issues.

## Success Criteria
- Lighthouse Performance score improves to ≥90 (or as high as possible given the static nature).
- The “Render Blocking Resources” audit shows zero high‑impact resources.
- Total font payload under 500 KB (down from 3.99 MB).
- Page loads completely within 3–5 seconds on a simulated Fast 3G connection.
