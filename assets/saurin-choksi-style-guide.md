# Saurin Choksi — Portfolio Style Guide
**Last Updated:** December 2025

---

## Brand Essence

**Positioning:** "Technical Designer / Bridging story + code"

**Tagline:** "I build the tools, pipelines, and prototypes for interactive storytelling. Before technical design, I wrote for Mo Willems and earned an Emmy nomination for Daniel Tiger's Neighborhood. With an understanding of story, systems, and audience, I now translate creative vision into engineering reality."
"

**Aesthetic:** Tactile Tech — warm, editorial, industrial warmth. Not "Hacker Dark Mode."

---

## Color Palette

| Name | Hex | RGB | Usage |
|------|-----|-----|-------|
| **Maroon** | `#6A3843` | 106, 56, 67 | Primary brand, headers, emphasis, links |
| **Antique Gold** | `#BFA265` | 191, 162, 101 | Accent, arrows, highlights, "What's Next" headers |
| **Dusty Rose** | `#F0E4E6` | 240, 228, 230 | Callout backgrounds, section backgrounds, dividers |
| **Cream** | `#FDFCFA` | 253, 252, 250 | Page background |
| **Warm White** | `#F5F3EF` | 245, 243, 239 | Secondary backgrounds (e.g., "Old Way" box) |
| **Text Dark** | `#3D3335` | 61, 51, 53 | Headings, primary body text |
| **Text Gray** | `#555555` | 85, 85, 85 | Body text, descriptions |
| **Text Light** | `#777777` | 119, 119, 119 | Labels, captions, metadata |

### Color Application

| Element | Color |
|---------|-------|
| Page background | Cream `#FDFCFA` |
| Header/Footer background | Maroon `#6A3843` |
| Header/Footer text | Warm White `#F5F3EF` |
| Header/Footer accent | Gold `#BFA265` |
| H1, H2 headings | Text Dark `#3D3335` |
| Body text | Text Gray `#555555` |
| Links | Maroon `#6A3843` |
| Link hover | Gold `#BFA265` |
| Pull quote text | Maroon `#6A3843` |
| Pull quote border | Gold `#BFA265` |
| Callout box background | Dusty Rose `#F0E4E6` |
| Impact section background | Dusty Rose `#F0E4E6` |
| Impact numbers (primary) | Maroon `#6A3843` |
| Impact numbers (alternate) | Gold `#BFA265` |
| Card background | White `#FFFFFF` |
| Card border | `rgba(106, 56, 67, 0.1)` |
| Card shadow | `rgba(106, 56, 67, 0.06)` |

---

## Typography

### Font Families

| Font | Type | Source | Usage |
|------|------|--------|-------|
| **Fraunces** | Serif | Google Fonts | Headlines, quotes, display text |
| **DM Sans** | Sans-serif | Google Fonts | Body text, UI elements, labels |

### Font Loading

**Code Injection → Header:**
```html
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@1,400&display=swap" rel="stylesheet">
```

**Squarespace Font Settings:**
- Headers: Fraunces 900
- Body: DM Sans 400

### Fraunces Weights

| Weight | Style | Usage |
|--------|-------|-------|
| 400 | Italic | Hero quote ("6 steps → 1"), testimonial blockquotes |
| 700 | Regular | H1, H2, section headlines |
| 700 | Italic | Pull quotes (e.g., "Playing audio files...") |
| 900 | Regular | Impact numbers, homepage hero |

### DM Sans Weights

| Weight | Usage |
|--------|-------|
| 400 | Body text, descriptions |
| 500 | Navigation, meta text |
| 600 | Labels, buttons, list item bold text |
| 700 | Strong emphasis (rare) |

### Type Scale

| Element | Font | Size (Mobile) | Size (Desktop) | Weight | Style |
|---------|------|---------------|----------------|--------|-------|
| Hero quote | Fraunces | 3rem | 5rem | 400 | Italic |
| H1 | Fraunces | 1.75rem | 2.25rem | 700 | Normal |
| H2 | Fraunces | 1.5rem | 1.75rem | 700 | Normal |
| Pull quote | Fraunces | 1.4rem | 1.85rem | 700 | Italic |
| Blockquote | Fraunces | 1.15rem | 1.35rem | 400 | Italic |
| Body | DM Sans | 1rem | 1.15rem | 400 | Normal |
| Labels | DM Sans | 0.75-0.8rem | 0.75-0.8rem | 600 | Uppercase |
| Meta | DM Sans | 0.9rem | 0.95rem | 400 | Normal |
| Impact numbers | Fraunces | 2.5rem | 3rem | 900 | Normal |

### Line Heights

| Element | Line Height |
|---------|-------------|
| Headlines | 1.1 - 1.2 |
| Pull quotes | 1.4 |
| Body text | 1.7 |
| Blockquotes | 1.6 |

### Letter Spacing

| Element | Letter Spacing |
|---------|----------------|
| Uppercase labels | 0.1em |
| Body text | Normal |

---

## Spacing

### Section Padding

| Breakpoint | Horizontal | Vertical |
|------------|------------|----------|
| Mobile | 1.5rem | 3rem |
| Desktop | 2rem | 3.5rem - 4rem |

### Content Max Width

| Element | Max Width |
|---------|-----------|
| Main content | 650px |
| Hero section | 700px |
| Impact section | 800px |

### Component Spacing

| Element | Margin/Padding |
|---------|----------------|
| Paragraph margin-bottom | 1.25rem |
| Section margin-bottom | 2rem |
| Card padding (mobile) | 1.5rem |
| Card padding (desktop) | 2rem - 2.5rem |
| Callout box padding | 1.5rem - 2.5rem |

---

## Components

### Cards

```css
background: white;
border: 1px solid rgba(106, 56, 67, 0.1);
border-radius: 8px;
box-shadow: 0 4px 24px rgba(106, 56, 67, 0.06);
padding: 2rem;
```

### Callout Boxes

```css
background: #F0E4E6;
border-radius: 8px;
padding: 1.75rem;
```

### Pull Quote / Blockquote Border

```css
border-left: 4px solid #BFA265;
padding-left: 1.25rem - 1.5rem;
```

### Labels (Uppercase)

```css
font-size: 0.75rem - 0.8rem;
font-weight: 600;
letter-spacing: 0.1em;
text-transform: uppercase;
color: #777 or #6A3843;
```

### Buttons / Step Boxes

```css
background: white;
padding: 0.5rem 0.75rem;
border-radius: 4px;
border: 1px solid #ddd;
font-weight: 500;
```

### Dividers

```css
border-bottom: 1px solid #F0E4E6;
```

---

## Imagery

### GIF / Demo Container

```css
background: linear-gradient(135deg, #3D3335 0%, #4a3f42 100%);
border-radius: 8px;
padding: 2rem;
```

### Image Shadows

```css
box-shadow: 0 4px 20px rgba(0,0,0,0.3);
border-radius: 4px;
```

---

## Responsive Breakpoints

| Name | Width |
|------|-------|
| Mobile | < 768px |
| Desktop | ≥ 768px |

---

## Accessibility

### Font Smoothing
```css
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
```

### Minimum Contrast
- Body text on cream: 4.5:1+ ✓
- Maroon on cream: 7:1+ ✓
- Gold on cream: Accent only (not for body text)

---

## Logo Usage

### Brand Logos Displayed (Homepage)
- Disney
- Netflix
- Apple TV+
- PBS Kids
- Sesame Workshop
- Amazon Kids+
- Nick Jr.

### Tech Stack Display
`PYTHON // JAVASCRIPT // ML // COMPUTER VISION // AUDIO // PERFORCE`

---

## Quick Reference

### CSS Variables (if implementing)

```css
:root {
  --maroon: #6A3843;
  --gold: #BFA265;
  --dusty-rose: #F0E4E6;
  --cream: #FDFCFA;
  --warm-white: #F5F3EF;
  --text-dark: #3D3335;
  --text-gray: #555555;
  --text-light: #777777;
}
```

---

## Files & Resources

### Font Import (Code Injection)
```html
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@1,400&display=swap" rel="stylesheet">
```

### Google Fonts URLs
- Fraunces: https://fonts.google.com/specimen/Fraunces
- DM Sans: https://fonts.google.com/specimen/DM+Sans

---

*Style guide for saurinchoksi.com*
