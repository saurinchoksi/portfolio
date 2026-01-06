# Project TODOs

The following items need manual attention (added Dec 2025 after technical audit):

### Critical (Before Deploy)
- [x] **Optimize hero image** - Compressed from 6.9MB to 187KB (resized to 1200px width)
- [x] **Optimize logo images** - Compressed from ~800KB to ~48KB total (resized to 200px width)
- [x] **Replace domain placeholders** - Updated to `saurinchoksi.com` in: `robots.txt`, `sitemap.xml`, and Open Graph tags in all HTML files
- [x] **Create og-image.jpg** - Created from hero image at 1200x630px, 146KB

### Mobile Issues (iPhone 16 Portrait)
- [x] **Tech stack font sizing** - Fixed: Reduced font to 0.55rem with tighter separator margins (0.3rem → 0.25rem at 480px) and added line-height: 1.8 for better two-row display.
- [x] **"5 Steps → 1" wrapping** - Fixed: Added `white-space: nowrap` to `.project-stat` at mobile breakpoints and increased font to 2.5rem (2.25rem at 480px) to maximize size while keeping on one line.

### Optional (Post-Deploy)
- [x] **Fix color contrast** - Gold hover states have 2.39:1 contrast ratio. Accepted as-is since hover is transient and default maroon links pass WCAG AA.
- [x] **Enable HSTS** - Added `_headers` file with Strict-Transport-Security header

---

## Sequence Expert Demo Enhancement

Replace the static mockup in `case-studies/sequence-expert.html` (lines 175-198) with a working demo.

### Phase 1: Video Recording (Quick Win) ✅
- [x] Record 10-15 second screen capture of actual Sequence Expert tool
- [x] Show: dropdown cascade → Play All → audio plays
- [x] Export as MP4, 1.5MB (under 2MB target)
- [x] Embed with video controls, poster image, and "🔊 Sound on" HTML overlay badge

### Phase 2: Interactive Demo (Future)
- [ ] Build embedded interactive demo with sample data
- [ ] See full spec: [`todo/sequence-expert-demo-spec.md`](sequence-expert-demo-spec.md)
