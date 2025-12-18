# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML/CSS portfolio website for Saurin Choksi, designed for deployment on GitHub Pages or Netlify. The site showcases professional work as a Technical Designer bridging storytelling and engineering.

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
│   ├── styles.css            # Single CSS file with design system
│   ├── images/               # Images and logos
│   └── saurin-choksi-style-guide.md  # Design specifications
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

## Styling Guidelines

### Component Classes

Key reusable components defined in `assets/styles.css`:

- `.project-card` - White cards with shadow (featured work)
- `.pull-quote` - Italic serif quotes with gold left border
- `.technical-section` - Dark gradient backgrounds for code/technical content
- `.impact-section` - Dusty rose background with metric cards
- `.label` - Uppercase small text for section labels
- `.hero-stat` - Large italic numbers for key metrics
- `.highlight-box` - Highlighted text with maroon border

If content would benefit from a new component type, make a reccomendation and ask the user before implementing.


### Responsive Breakpoints

- Mobile: `< 768px`
- Desktop: `≥ 768px`

Mobile-first approach with desktop overrides.

### Color Usage

| Element | Variable |
|---------|----------|
| Headings | `var(--text-dark)` |
| Body text | `var(--text-read)` |
| Links | `var(--maroon)` |
| Link hover | `var(--gold)` |
| Page background | `var(--cream)` |
| Header/Footer | `var(--maroon)` background |
| Accents | `var(--gold)` |

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

## Reference Files

- `assets/saurin-choksi-style-guide.md` - Complete design specifications including exact font sizes, weights, spacing

## Remaining TODOs

See [todo/TODO.md](todo/TODO.md) for the current project status and remaining tasks.

