# Persona.com.ge – Agent Memory

This file contains repository‑specific knowledge, performance optimizations, and key versions for the `TP1M.github.io` site (live at **https://persona.com.ge**).

---

## 🚀 **Performance‑Optimized Version (`main‑version` tag)**

**Tag:** `main‑version`  
**Commit:** `9b242c6` (18 April 2026)  
**Status:** Production‑ready baseline

### 🔧 **Optimizations Applied**

| **Component** | **Change** | **Impact** |
|---------------|------------|------------|
| **Material Symbols** | Subset via `&text=sunnydark_mode` + `media="print" onload="this.media='all'"` | 3.73 MB → ~10 KB, non‑render‑blocking |
| **GSAP & ScrollTrigger** | Added `defer` attribute | Animations load after DOM, no render‑block |
| **Font Awesome** | `media="print" onload="this.media='all'"` | CSS loads in background, switches when ready |
| **Polyfill.io** | Removed entirely | –200 KB, –1 RTT (unnecessary for modern browsers) |
| **Google Fonts** | Already have `display=swap` | No FOIT |

### 📝 **Content Updates (Latest)**

1. **Service block “ვებ‑საიტის/გვერდის შექმნა”**
   - Removed “ახალი” badge
   - Heading renamed from `ვებ/ლენდინგ გვერდის შექმნა`

2. **Service block “იდეიდან ბიზნესამდე”**
   - Price changed from `2,500 – 7,000 ₾` to **“შეთანხმებით”**
   - “ახალი” badge color changed to **red** (`bg‑red‑500 text‑white`)

3. **Grammar fix**
   - `დომენი რეგისტრაცია` → `დომენის რეგისტრაცია`

4. **Dropdown option** updated to match new heading.

### 🎯 **Load‑Time Target**
- **Goal:** ≈1‑1.5 seconds (down from ~5 seconds)
- **Critical path:** No render‑blocking resources >100 KB
- **Font stack:** Google Fonts (Inter + Space Grotesk) already subsetted & self‑hosted

### 🔄 **How to Restore This Version**

```bash
# Checkout the tagged version
git checkout main-version

# Or create a branch from it
git checkout -b restore-main-version main-version

# View the tag details
git show main-version
```

### 📈 **Monitoring**
- **Live URL:** https://persona.com.ge
- **GitHub Pages:** https://TP1M.github.io
- **Performance audits:** Run Lighthouse (Chrome DevTools)

---

## 🧪 **Live Experiment: Hamburger Menu & Version Switcher**

**Commit:** `fed4076` (18 April 2026)  
**Status:** Active experiment on `main` branch with complete New Design 1

### 🔧 **What's New**
| **Feature** | **Description** |
|-------------|-----------------|
| **Hamburger menu** | Mobile‑only toggle (≤768px) that reveals social buttons, version switcher, and dark‑mode toggle |
| **Version switcher** | Button in header toggles between **Original** (v1) and **New Design 1** (v2) |
| **New theme** | `theme‑new‑design‑1` CSS class with complete business minimalistic redesign |
| **Dark‑mode support** | Full dark mode compatibility with adjusted colors |
| **Persistence** | User's version preference stored in `localStorage` (`siteVersion`) |

### 🎨 **New Design 1: Business Minimalistic**
**Color Palette:**
- **Primary:** `#1e3a8a` (navy blue)
- **Accent:** `#0ea5e9` (sky blue)
- **Backgrounds:** Solid white (`#ffffff`) / dark blue (`#0f172a`)
- **Cards:** Clean white with subtle borders and shadows

**Key Design Changes:**
1. **Hero section** – Solid backgrounds, decorative gradients and shapes removed
2. **Typography** – Gradient text replaced with solid colors
3. **Buttons** – Solid colors with subtle shadows, rounded corners
4. **Service cards** – Minimalistic design with subtle borders and shadows
5. **Service icons** – Simplified backgrounds
6. **Contact form** – Clean borders with focus states
7. **Navigation** – Clean glass effect with solid background
8. **Process steps** – Simplified circles and lines
9. **All decorative gradients** removed or simplified

**Layout:** Unchanged – same HTML structure, Tailwind classes, and GSAP animations.

### 🔄 **How to Toggle**
1. **On desktop:** Click the version switcher button (right of social icons)
2. **On mobile:** Open hamburger menu, then tap version switcher
3. **State persists** across page reloads

### ⚠️ **Reverting to Original**
- The `main‑version` tag remains untouched (`git checkout main‑version`)
- To remove experiment entirely:
  ```bash
  git checkout main-version -- index.html
  ```
- Or toggle back to **Original** using the version switcher button

### 📱 **Mobile Behavior**
- Hamburger icon appears only on small screens
- Clicking toggles a dropdown with all header actions
- Menu closes when clicking outside or selecting an item

---

## 🗂️ **Repository Structure**

```
TP1M.github.io/
├── index.html              # Single‑page site (Tailwind, GSAP, Font Awesome)
├── _headers                # Netlify‑style cache headers (optional)
├── fonts/                  # Self‑hosted subsetted Google Fonts
│   ├── inter-subset.woff2
│   └── space-grotesk-subset.woff2
└── AGENTS.md              # This file
```

**Note:** The site uses CDN for Tailwind CSS (`cdn.tailwindcss.com`), Google Fonts (subsetted), and GSAP. All other resources are optimized for minimal render‑blocking.

---

## 📚 **Development Notes**

- **No build process** – edits go directly to `index.html`
- **Tailwind classes** are used inline; no separate CSS file
- **Dark‑mode toggle** uses Material Symbols (`sunny` / `dark_mode`) with subsetted font
- **Form submission** handled by Formspree (ID: `xeqywrpo`)
- **Animations** powered by GSAP + ScrollTrigger (deferred)

---

*This file is maintained by OpenHands agent to preserve context across sessions.*  
*Last updated: 18 April 2026*