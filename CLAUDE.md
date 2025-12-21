# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML/CSS portfolio website for Saurin Choksi, designed for deployment on **Netlify** (with optional GitHub Pages support). The site showcases professional work as a Technical Designer bridging storytelling and engineering.

**Live Site**: https://saurinchoksi.com
**Tech Stack**: Pure HTML/CSS (no build process), Google Fonts (Fraunces + DM Sans)
**Deployment**: Auto-deploy via Netlify on push to main branch

## Architecture

### Design System Foundation

This portfolio implements a **CSS Variable-based design system** defined in `assets/styles.css`. All styling must use these system tokens rather than hardcoded values:

- **Colors**: Use CSS variables like `var(--maroon)`, `var(--gold)`, `var(--dusty-rose)`, etc. Never use raw hex codes.
- **Typography**: Use predefined size tokens like `var(--font-size-h1)`, `var(--font-size-body)`, etc. Never use arbitrary font sizes.
- **Fonts**: Fraunces (serif, for headings/quotes) and DM Sans (sans-serif, for body text)

**Critical Rule**: When implementing designs, always map values to existing system variables. If a design requires a new color or size not in the system, flag it to the user first.

### File Structure

```
/
├── index.html                 # Homepage with hero, logo bar, featured work
├── about.html                 # About page with header grid layout
├── contact.html               # Contact page with form
├── case-studies/
│   └── sequence-expert.html  # Case study template/example
├── assets/
│   ├── styles.css            # Single CSS file with design system (1600+ lines)
│   ├── images/               # Optimized images and logos
│   │   ├── choksi-home.webp  # Hero image (191KB, optimized)
│   │   ├── choksi-about.webp # About page image (267KB, optimized)
│   │   ├── og-image.jpg      # Open Graph social share image (1200x630)
│   │   ├── favicon/          # Multi-platform favicon files
│   │   └── logos/            # Client logo images (optimized to ~48KB total)
│   └── saurin-choksi-style-guide.md  # Design specifications
├── netlify.toml              # Netlify config (security headers, caching, redirects)
├── _headers                  # HSTS header configuration
├── _redirects                # URL redirects and privacy rules (hide /todo, etc.)
├── _config.yml               # Jekyll config for GitHub Pages (excludes CLAUDE.md, todo/)
├── robots.txt                # SEO: Allow all crawlers
├── sitemap.xml               # SEO: All pages with priority weights
└── todo/                     # Project planning and specs (excluded from deploy)
    ├── TODO.md               # Current project status and tasks
    └── *.md                  # Design specs and planning docs
```

### Page Patterns

1. **Homepage (`index.html`)**: Two-column hero grid (image + content), full-width logo bar, centered featured work cards
2. **About Page (`about.html`)**: Header grid (text + image), pull quotes, icon lists
3. **Contact Page (`contact.html`)**: Two-column layout (contact info + form)
4. **Case Studies**: Hero stats, problem/solution sections, technical sections with dark gradient backgrounds, impact metrics

## Development Workflow

### No Build Process

This is a static site with no build step. All changes are direct HTML/CSS edits.

**Local Development**:
```bash
# Serve locally (Python 3)
python3 -m http.server 8000

# Or use any static file server
npx http-server
```

**Deployment**:
```bash
# Push to GitHub (Netlify auto-deploys on push)
git add .
git commit -m "Update content"
git push
```

### Deployment & Configuration

**Netlify Configuration (`netlify.toml`)**:
- **Security Headers**: CSP, X-Frame-Options, X-Content-Type-Options, XSS Protection, Referrer Policy, Permissions Policy
- **Caching Strategy**: `max-age=0, must-revalidate` for all assets (HTML, CSS, images) to prevent stale content
- **Cache Busting**: CSS files use query param versioning (e.g., `styles.css?v=2`) - increment version after CSS changes
- **URL Redirects**: `/portfolio` → `/case-studies/sequence-expert.html` (301 redirect)

**Additional Configuration Files**:
- `_headers`: HSTS (Strict-Transport-Security) header for HTTPS enforcement
- `_redirects`: Privacy rules (404 for `/CLAUDE.md`, `/.gitignore`, `/todo/*`)
- `_config.yml`: Jekyll excludes for GitHub Pages deployment

**Important**: After CSS changes, increment the version number in all HTML `<link>` tags:
```html
<link rel="stylesheet" href="assets/styles.css?v=3">  <!-- Increment v=2 → v=3 -->
```

### SEO & Metadata

All HTML pages include:
- **Title tags** optimized for search
- **Meta descriptions** (155-160 characters)
- **Canonical URLs** pointing to https://saurinchoksi.com
- **Open Graph tags** (og:title, og:description, og:image, og:url, og:type)
- **Twitter Card tags** for social sharing
- **Favicons** for multiple platforms (96x96 PNG, SVG, ICO, Apple Touch Icon, web manifest)

**Sitemap** (`sitemap.xml`):
- Homepage: Priority 1.0
- Case studies: Priority 0.9
- About: Priority 0.8
- Contact: Priority 0.6

**Robots.txt**: Allows all crawlers, references sitemap

### Accessibility

Built-in accessibility features:
- **Skip link**: "Skip to main content" link for keyboard navigation (visible on focus)
- **Semantic HTML**: Proper heading hierarchy, `<main>`, `<nav>`, `<header>`, `<footer>`
- **Focus indicators**: 3px gold outline on all interactive elements for keyboard users
- **Alt text**: All images have descriptive alt attributes
- **ARIA attributes**: Where needed for complex interactions
- **No WCAG violations**: Gold hover states (2.39:1 contrast) accepted as transient interactions

### Design System Protocol

Follow the "System First" approach:

1. **Map to existing tokens**: Convert design values to CSS variables
2. **Never hardcode**: Use `var(--maroon)` not `#6A3843`
3. **Flag deviations**: If a design introduces new colors/sizes, ask the user before implementing

### Adding New Content

**New Case Study**:
1. Duplicate `case-studies/sequence-expert.html`
2. Update content following existing structure patterns
3. Add card to `index.html` Featured Work section

**New Page**:
1. Copy header/footer from existing pages
2. Use `<main>` with appropriate max-width class
3. Follow component patterns from `assets/styles.css`
4. Add to `sitemap.xml` with appropriate priority
5. Update Open Graph metadata (title, description, URL, image)

### Git Workflow & Commit Conventions

**Commit Message Format**:
```
type(scope): brief description

Examples:
- fix(mobile): prevent text orphans in tech stack
- feat(case-study): add new project to portfolio
- chore(deps): update Google Fonts
- fix(ops): force cache bust with version query param
- docs: update CLAUDE.md with deployment info
```

**Common Types**:
- `fix`: Bug fixes (UI issues, layout problems, content errors)
- `feat`: New features (new case studies, pages, components)
- `chore`: Maintenance (dependencies, config updates)
- `docs`: Documentation changes
- `refactor`: Code restructuring without behavior change
- `style`: CSS-only changes
- `perf`: Performance improvements

**Common Scopes**:
- `mobile`: Mobile-specific fixes
- `ops`: Deployment, caching, infrastructure
- `a11y`: Accessibility improvements
- `seo`: SEO-related changes

## Styling Guidelines

### Component Classes

Key reusable components defined in `assets/styles.css` (see file for complete list):

**Layout & Structure**:
- `.home-hero` - Two-column hero grid (image + content)
- `.hero-image` / `.hero-content` - Hero grid children
- `.logo-bar` - Full-width client logo showcase
- `.featured-work` - Featured work cards container
- `.contact-container` - Two-column contact layout
- `.about-header-section` - About page header grid

**Cards & Content Blocks**:
- `.project-card` - White cards with shadow (featured work)
- `.impact-card` - Metric cards with numbers and descriptions
- `.demo-card` - Demo/mockup containers
- `.highlight-box` - Highlighted text with maroon border and gold left border

**Typography & Text**:
- `.pull-quote` - Italic serif quotes with gold left border
- `.label` - Uppercase small text (0.75rem, 0.1em letter-spacing)
- `.label--gold` - Gold variant of label
- `.hero-stat` - Large italic numbers (4rem Fraunces)
- `.impact-number` - Impact metric numbers
- `.meta` - Metadata display (dates, roles, etc.)
- `.keyword` - Highlighted inline terms

**Sections & Backgrounds**:
- `.technical-section` - Dark gradient backgrounds for code/technical content
- `.impact-section` - Dusty rose background with metric cards
- `.solution-section` - Solution narrative sections
- `.backend-section` - Backend/technical implementation sections

**Special Components**:
- `.tech-stack` - Technology list with separators
- `.icon-list` / `.icon-item` - Icon-based feature lists
- `.friction-list` - Problem statement lists
- `.pipeline-flow` - Technical pipeline diagrams
- `.code-block` - Code syntax highlighting containers
- `.testimonial` - Quote blocks with attribution

**Navigation**:
- `.skip-link` - Keyboard navigation skip link (visible on focus)
- `.back-link` - "← Back to Work" style links
- `.cta-link` - Call-to-action links

**Important**: Before creating new component classes, check if an existing component can be reused or extended. If content would benefit from a new component type, make a recommendation and ask the user before implementing.


### Responsive Breakpoints

- Mobile: `< 768px`
- Desktop: `≥ 768px`
- Extra small mobile: `< 480px` (additional refinements for iPhone SE, etc.)

**Mobile-First Approach**: All base styles target mobile. Desktop styles use `@media (min-width: 768px)` overrides.

**Critical Mobile Patterns**:
- **No wrapping on stats**: Use `.no-wrap` class and `white-space: nowrap` for key metrics that must stay on one line
- **Font scaling**: Reduce font sizes at 480px for tight layouts (tech stack, project stats)
- **Grid collapsing**: Two-column desktop layouts → single column on mobile
- **Touch targets**: Minimum 44x44px for buttons and links (iOS guidelines)
- **Line-height for readability**: Increase line-height on smaller text to prevent crowding

**Testing Priority**: Test on real iOS devices (Safari engine behaves differently than Chrome DevTools). Recent fixes focused on iPhone 16 portrait mode.

### CSS Variables Reference

**Brand Colors**:
- `--maroon: #6A3843` - Primary brand color (headings, header/footer, links)
- `--gold: #BFA265` - Accent color (hover states, borders, highlights)
- `--gold-dark: #a88d55` - Darker gold variant
- `--dusty-rose: #F0E4E6` - Light accent (borders, section backgrounds)
- `--cream: #FDFCFA` - Page background
- `--warm-white: #F5F3EF` - Card/section backgrounds
- `--pure-white: #ffffff` - Pure white for cards

**Text Colors**:
- `--text-dark: #3D3335` - Headings
- `--text-read: #4A4345` - Body text (optimized for reading)
- `--text-light: #777777` - Meta text, captions
- `--text-light-on-dark: rgba(255, 255, 255, 0.85)` - Text on dark backgrounds

**Dark Section Gradient**:
- `--charcoal-start: #3D3335` → `--charcoal-end: #4a3f42`

**Typography Tokens**:
- `--font-size-display: 3rem` - Hero H1
- `--font-size-h1: 2.25rem` - Standard H1
- `--font-size-h2: 1.75rem` - Standard H2
- `--font-size-h3: 1.5rem` - Card headers
- `--font-size-body: 1.1rem` - Standard body text
- `--font-size-small: 0.95rem` - Meta, nav
- `--font-size-label: 0.75rem` - Uppercase labels
- `--font-size-stat: 4rem` - Impact numbers

**Shadows & Borders**:
- `--card-shadow: 0 4px 24px rgba(106, 56, 67, 0.08)` - Card drop shadows
- `--card-border: rgba(106, 56, 67, 0.08)` - Card borders
- `--border-light: #dddddd` - Light borders
- `--border-lighter: #d4d4d4` - Even lighter borders

### Color Usage Patterns

| Element | Variable |
|---------|----------|
| Headings | `var(--text-dark)` |
| Body text | `var(--text-read)` |
| Links | `var(--maroon)` |
| Link hover | `var(--gold)` |
| Page background | `var(--cream)` |
| Header/Footer | `var(--maroon)` background |
| Accents | `var(--gold)` |

## Performance & Optimization

### Image Optimization

All images have been optimized for web performance:
- **Hero images**: WebP format, resized to 1200px width, compressed (choksi-home.webp: 191KB, choksi-about.webp: 267KB)
- **Logo images**: Resized to 200px width, total ~48KB for all logos
- **Open Graph image**: JPG format, 1200x630px, 146KB
- **Favicon**: Multiple formats and sizes for cross-platform support

**Image Guidelines**:
- Use WebP for photos (better compression than JPG)
- Target max 200-300KB per image
- Resize to actual display size (2x for retina: display 600px = 1200px source)
- Always include `width` and `height` attributes to prevent layout shift
- Use descriptive `alt` text for accessibility

### Performance Best Practices

- **No JavaScript**: Pure HTML/CSS site loads instantly
- **Google Fonts**: Preconnect to fonts.googleapis.com and fonts.gstatic.com
- **Cache busting**: Version CSS files (?v=N) when making changes
- **Minimal HTTP requests**: Single CSS file, optimized images
- **Lazy loading**: Consider adding `loading="lazy"` to below-fold images

## Important Context

### Brand Identity

The site reflects a unique "Tactile Tech" aesthetic - warm, editorial, and industrial warmth (not typical "Hacker Dark Mode"). This influences:
- Color palette (maroon/gold over blue/purple)
- Typography (serif headings, warm neutrals)
- Spacing (generous padding, breathing room)

### Key Design Patterns

1. **Stats/Metrics**: Use large italic Fraunces font (`hero-stat`, `project-stat`)
2. **Section Dividers**: 1px solid `var(--dusty-rose)` borders
3. **Dark Sections**: Use gradient from `--charcoal-start` to `--charcoal-end`
4. **Labels**: Always uppercase, 0.1em letter-spacing, 0.75rem font size

### Content Strategy

Portfolio focuses on:
- Process over just outcomes
- Technical depth in case studies
- Clear before/after value propositions
- Industry experience (Disney, Netflix, PBS Kids, etc.)

## Common Tasks & Troubleshooting

### CSS Not Updating After Deploy

**Problem**: CSS changes don't appear after pushing to Netlify
**Solution**:
1. Increment version number in all HTML files: `styles.css?v=2` → `styles.css?v=3`
2. Clear browser cache or test in incognito mode
3. Check Netlify deploy log to confirm CSS file was updated

### Adding a New Case Study

**Checklist**:
- [ ] Duplicate `case-studies/sequence-expert.html` and rename
- [ ] Update all content (title, meta tags, Open Graph tags, canonical URL)
- [ ] Optimize and add images to `assets/images/`
- [ ] Add project card to `index.html` Featured Work section
- [ ] Add URL to `sitemap.xml` (priority: 0.9)
- [ ] Increment CSS version if styles changed
- [ ] Test on mobile (especially iPhone Safari)
- [ ] Commit with message: `feat(case-study): add [project name]`

### Testing Mobile Layouts

**Priority Devices**:
- iPhone 16 (portrait mode) - 393×852px
- iPhone SE (smallest iOS device) - 375×667px
- Desktop - 1440px+ width

**Common Mobile Issues**:
- Text wrapping on stats: Add `.no-wrap` class
- Font too large: Check media query at `@media (max-width: 480px)`
- Grid not collapsing: Verify `grid-template-columns: 1fr` on mobile breakpoint
- Touch targets too small: Ensure minimum 44×44px

### Accessibility Checks

**Manual Checks**:
- Tab through page with keyboard - focus indicators visible?
- All images have descriptive alt text?
- Heading hierarchy logical (h1 → h2 → h3)?
- Skip link works (press Tab on page load)?
- Color contrast meets WCAG AA (4.5:1 for normal text)?

## Reference Files

- `assets/saurin-choksi-style-guide.md` - Complete design specifications including exact font sizes, weights, spacing
- `netlify.toml` - Deployment configuration, security headers, caching rules
- `todo/TODO.md` - Current project status and remaining tasks
- `todo/*.md` - Design specs and planning documents

## Remaining TODOs

See [todo/TODO.md](todo/TODO.md) for the current project status and remaining tasks.

