# Persona.com.ge – Agent Memory

This file contains repository‑specific knowledge, performance optimizations, and key versions for the `TP1M.github.io` site (live at **https://persona.com.ge**). Also includes agent speed/efficiency rules.

---

## ⚡ Speed & Efficiency Rules (for OpenHands)

### 1. Batch operations over single edits
- Combine multiple sed/grep operations into one command
- Edit all similar files at once: `sed -i 's/pattern/replacement/g' *.html`
- Use `&&` to chain commands

### 2. Read first, edit later
- Run `find` + `grep -rn` before any edits — understand full picture first
- Then make one batch of edits — no explore→edit ping-pong

### 3. No speculative work
- Don't explore unless preparing an edit
- Don't run tests unless verifying a change
- Don't install deps unless needed

### 4. Commit discipline
- One commit per logical change: `git add -A && git commit -m "summary"`
- Include `Co-authored-by: openhands <openhands@all-hands.dev>`

### 5. File operations
- Prefer `sed -i` for global search-and-replace over file_editor
- Use `cat -n file | head -30` for quick previews
- Use `grep -n` to find exact line numbers

### 6. Communication
- "why X happens" → just answer, don't fix
- Discussion → don't make edits, just talk
- Stuck after 2 attempts → stop and suggest new plan

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

### 📝 **Latest Edits (April 2026)**

1. **Hero title:** Changed from `"თქვენი ბიზნესის / ონლაინ მხარდაჭერა"` → `"შენი პირადი / ონლაინ ასისტენტი"`
2. **Service card arrows:** Removed all `contact-service-btn` arrow buttons + CSS
3. **Hero background:** Replaced Font Awesome icons with stroked squares (no fill, rotated, varied sizes)
4. **Messenger icons:** Added brand colors at 50% transparency + material shadow effect (`var(--shadow-sm)` / `--shadow-md` on hover)
5. **Microsoft Clarity:** Added tracking script (`wg2oipqny2`) to `<head>`
6. **Text update:** `"ვებ‑საიტის/გვერდის"` → `"ვებ‑საიტის/მაღაზიის"` (service card + dropdown)
7. **Phone link:** Footer phone → `<a href="tel:...">` (clickable)
8. **Grammar fix:** `დომენი რეგისტრაცია` → `დომენის რეგისტრაცია`

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

## 🎨 **New Design 1: Business Minimalistic (Now Default)**

**Commit:** `f5cded9` (18 April 2026)  
**Status:** Permanent default design on `main` branch

### 🎨 **Color Palette**
- **Primary:** `#1e3a8a` (navy blue)
- **Accent:** `#0ea5e9` (sky blue)
- **Backgrounds:** Solid white (`#ffffff`) / dark blue (`#0f172a`)
- **Cards:** Clean white with subtle borders and shadows

### 🔧 **Key Design Changes**
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

### 📱 **Mobile Experience**
- **Hamburger menu** for mobile (≤768px) reveals social buttons and dark‑mode toggle
- **Version switcher removed** – New Design 1 is now the permanent default
- **Hamburger menu functionality retained** for mobile usability

### ⚠️ **Reverting to Original Design**
- The `main‑version` tag remains untouched (`git checkout main‑version`)
- To restore original design:
  ```bash
  git checkout main-version -- index.html
  ```

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