# Project TODOs

The following items need manual attention (added Dec 2025 after technical audit):

### Critical (Before Deploy)
- [x] **Optimize hero image** - Compressed from 6.9MB to 187KB (resized to 1200px width)
- [x] **Optimize logo images** - Compressed from ~800KB to ~48KB total (resized to 200px width)
- [x] **Replace domain placeholders** - Updated to `saurinchoksi.com` in: `robots.txt`, `sitemap.xml`, and Open Graph tags in all HTML files
- [x] **Create og-image.jpg** - Created from hero image at 1200x630px, 146KB

### Optional (Post-Deploy)
- [x] **Fix color contrast** - Gold hover states have 2.39:1 contrast ratio. Accepted as-is since hover is transient and default maroon links pass WCAG AA.
- [x] **Enable HSTS** - Added `_headers` file with Strict-Transport-Security header
