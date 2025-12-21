# Test Coverage Analysis & Recommendations

**Date**: 2025-12-21
**Status**: No testing infrastructure exists
**Site Type**: Static HTML/CSS portfolio (no JavaScript)

---

## Executive Summary

The portfolio currently has **zero automated testing infrastructure**. While this is common for simple static sites, implementing strategic testing would:
- Prevent regressions during updates
- Ensure accessibility compliance
- Validate SEO optimization
- Catch broken links before deployment
- Verify mobile responsiveness
- Monitor performance metrics

---

## Current State

### ✅ What Exists
- 4 production HTML pages (`index.html`, `about.html`, `contact.html`, `case-studies/sequence-expert.html`)
- 1 CSS file with design system (`assets/styles.css` - 1600+ lines)
- Netlify deployment configuration with security headers
- Manual testing workflow (mentioned in CLAUDE.md)

### ❌ What's Missing
- No test files or test framework configuration
- No CI/CD testing pipeline
- No automated validation
- No visual regression testing
- No accessibility audits
- No link checking
- No performance monitoring

---

## Recommended Testing Strategy

### Priority 1: Critical Tests (High Value, Low Effort)

#### 1. **HTML Validation**
**Why**: Catches syntax errors, missing tags, invalid attributes
**Impact**: Prevents browser rendering issues and SEO problems
**Tool**: [html-validate](https://html-validate.org/)

```bash
npm install --save-dev html-validate
```

**What to test**:
- Valid HTML5 syntax on all pages
- Required meta tags present (title, description, viewport)
- Proper semantic structure (header, main, footer, nav)
- No duplicate IDs
- All images have alt attributes
- All links have href attributes

**Test file example** (`tests/html-validation.test.js`):
```javascript
const { HtmlValidate } = require('html-validate');
const fs = require('fs');
const glob = require('glob');

describe('HTML Validation', () => {
  const htmlvalidate = new HtmlValidate();
  const htmlFiles = glob.sync('**/*.html', {
    ignore: ['node_modules/**', 'todo/**']
  });

  htmlFiles.forEach((file) => {
    it(`${file} should be valid HTML`, () => {
      const html = fs.readFileSync(file, 'utf8');
      const report = htmlvalidate.validateString(html);
      expect(report.valid).toBe(true);
    });
  });
});
```

---

#### 2. **CSS Validation**
**Why**: Catches syntax errors, invalid properties, typos in CSS variables
**Impact**: Prevents styling bugs and browser incompatibilities
**Tool**: [stylelint](https://stylelint.io/)

```bash
npm install --save-dev stylelint stylelint-config-standard
```

**What to test**:
- Valid CSS syntax
- No undefined CSS variables (e.g., `var(--typo-color)`)
- Color values match design system tokens
- No hardcoded colors (should use CSS variables)
- Font sizes use design system tokens
- No duplicate selectors

**Test configuration** (`.stylelintrc.json`):
```json
{
  "extends": "stylelint-config-standard",
  "rules": {
    "color-named": "never",
    "color-no-hex": true,
    "font-size-no-arbitrary": true,
    "custom-property-pattern": "^(maroon|gold|cream|text|font|card)-.+"
  }
}
```

**Critical for your codebase**: The design system protocol in CLAUDE.md states "Never hardcode colors" - this would enforce that rule automatically.

---

#### 3. **Link Checking**
**Why**: Broken links damage SEO and user experience
**Impact**: Catch 404s before users do
**Tool**: [broken-link-checker](https://www.npmjs.com/package/broken-link-checker)

```bash
npm install --save-dev broken-link-checker
```

**What to test**:
- All internal links resolve (`href="about.html"` → `/about.html` exists)
- All external links return 200 status (Google Fonts, social links, etc.)
- All image src paths exist
- CSS file referenced in `<link>` tags exists
- Favicon files exist

**Test file example** (`tests/links.test.js`):
```javascript
const blc = require('broken-link-checker');

test('All links are valid on homepage', async () => {
  const brokenLinks = [];
  const checker = new blc.HtmlChecker(options, {
    link: (result) => {
      if (result.broken) {
        brokenLinks.push(result.url.resolved);
      }
    }
  });

  await checker.scan('http://localhost:8000/index.html');
  expect(brokenLinks).toEqual([]);
});
```

**Critical checks**:
- `/portfolio` redirect to `/case-studies/sequence-expert.html` works
- All logo images in logo bar exist
- Hero images (`choksi-home.webp`, `choksi-about.webp`) exist
- Favicon files exist at referenced paths

---

#### 4. **Accessibility (A11y) Testing**
**Why**: Legal compliance (ADA), better UX, improved SEO
**Impact**: Ensure WCAG 2.1 AA compliance
**Tool**: [axe-core](https://github.com/dequelabs/axe-core) via [Pa11y](https://pa11y.org/)

```bash
npm install --save-dev pa11y
```

**What to test**:
- Color contrast ratios meet WCAG AA (4.5:1 for normal text, 3:1 for large text)
- All images have alt attributes
- Proper heading hierarchy (no skipped levels: h1 → h3)
- Form inputs have associated labels (on contact page)
- Skip link is present and functional
- Focus indicators visible on all interactive elements
- ARIA attributes used correctly

**Test file example** (`tests/accessibility.test.js`):
```javascript
const pa11y = require('pa11y');

test('Homepage passes WCAG AA accessibility', async () => {
  const results = await pa11y('http://localhost:8000/index.html', {
    standard: 'WCAG2AA'
  });
  expect(results.issues.length).toBe(0);
});

test('Contact form is accessible', async () => {
  const results = await pa11y('http://localhost:8000/contact.html');
  const formIssues = results.issues.filter(issue =>
    issue.selector.includes('form') || issue.selector.includes('input')
  );
  expect(formIssues.length).toBe(0);
});
```

**Critical for your codebase**: CLAUDE.md mentions "No WCAG violations" as a feature - this would verify that claim automatically.

---

### Priority 2: Important Tests (High Value, Medium Effort)

#### 5. **SEO Validation**
**Why**: Ensure proper indexing and social sharing
**Impact**: Better search rankings and social media previews
**Tool**: Custom Jest tests

**What to test**:
- Every page has unique `<title>` tag
- Every page has `<meta name="description">` (155-160 chars)
- Every page has canonical URL
- Open Graph tags present and valid (og:title, og:description, og:image, og:url, og:type)
- Twitter Card tags present
- `sitemap.xml` includes all production pages
- `robots.txt` allows crawlers
- All Open Graph images exist and are correct dimensions (1200x630)

**Test file example** (`tests/seo.test.js`):
```javascript
const cheerio = require('cheerio');
const fs = require('fs');

describe('SEO Meta Tags', () => {
  const pages = ['index.html', 'about.html', 'contact.html', 'case-studies/sequence-expert.html'];

  pages.forEach((page) => {
    describe(page, () => {
      let $;

      beforeAll(() => {
        const html = fs.readFileSync(page, 'utf8');
        $ = cheerio.load(html);
      });

      it('has unique title tag', () => {
        const title = $('title').text();
        expect(title).toBeTruthy();
        expect(title.length).toBeGreaterThan(10);
      });

      it('has meta description between 155-160 chars', () => {
        const description = $('meta[name="description"]').attr('content');
        expect(description.length).toBeGreaterThanOrEqual(155);
        expect(description.length).toBeLessThanOrEqual(160);
      });

      it('has canonical URL', () => {
        const canonical = $('link[rel="canonical"]').attr('href');
        expect(canonical).toContain('https://saurinchoksi.com');
      });

      it('has Open Graph tags', () => {
        expect($('meta[property="og:title"]').attr('content')).toBeTruthy();
        expect($('meta[property="og:description"]').attr('content')).toBeTruthy();
        expect($('meta[property="og:image"]').attr('content')).toBeTruthy();
        expect($('meta[property="og:url"]').attr('content')).toBeTruthy();
      });

      it('Open Graph image exists and is 1200x630', () => {
        const ogImage = $('meta[property="og:image"]').attr('content');
        const imagePath = ogImage.replace('https://saurinchoksi.com/', '');
        expect(fs.existsSync(imagePath)).toBe(true);

        // Use image-size library to check dimensions
        const sizeOf = require('image-size');
        const dimensions = sizeOf(imagePath);
        expect(dimensions.width).toBe(1200);
        expect(dimensions.height).toBe(630);
      });
    });
  });
});

test('Sitemap includes all pages', () => {
  const sitemap = fs.readFileSync('sitemap.xml', 'utf8');
  expect(sitemap).toContain('https://saurinchoksi.com/');
  expect(sitemap).toContain('https://saurinchoksi.com/about.html');
  expect(sitemap).toContain('https://saurinchoksi.com/contact.html');
  expect(sitemap).toContain('https://saurinchoksi.com/case-studies/sequence-expert.html');
});
```

**Critical issue to catch**: If you add a new case study page but forget to update `sitemap.xml`, this test would fail.

---

#### 6. **CSS Design System Compliance**
**Why**: Enforce "System First" protocol from CLAUDE.md
**Impact**: Prevent hardcoded values, ensure brand consistency
**Tool**: Custom CSS parser tests

**What to test**:
- No hardcoded hex colors in HTML (should use CSS variables)
- No hardcoded hex colors in CSS (except in variable definitions)
- No arbitrary font sizes (should use design system tokens)
- All CSS variables follow naming convention
- No duplicate CSS variable definitions
- CSS file uses consistent spacing/formatting

**Test file example** (`tests/design-system.test.js`):
```javascript
const fs = require('fs');
const css = require('css');

describe('Design System Compliance', () => {
  let stylesheet;

  beforeAll(() => {
    const cssContent = fs.readFileSync('assets/styles.css', 'utf8');
    stylesheet = css.parse(cssContent);
  });

  it('has no hardcoded hex colors outside :root', () => {
    const hexPattern = /#[0-9A-Fa-f]{3,6}/g;
    const rules = stylesheet.stylesheet.rules.filter(rule => {
      return rule.type === 'rule' && rule.selectors[0] !== ':root';
    });

    rules.forEach(rule => {
      rule.declarations.forEach(decl => {
        if (decl.value && decl.value.match(hexPattern)) {
          fail(`Hardcoded color found: ${decl.property}: ${decl.value} (use CSS variable instead)`);
        }
      });
    });
  });

  it('defines all required brand color variables', () => {
    const cssContent = fs.readFileSync('assets/styles.css', 'utf8');
    const requiredColors = [
      '--maroon', '--gold', '--gold-dark', '--dusty-rose',
      '--cream', '--warm-white', '--pure-white',
      '--text-dark', '--text-read', '--text-light'
    ];

    requiredColors.forEach(color => {
      expect(cssContent).toContain(`${color}:`);
    });
  });

  it('all font sizes use design system tokens', () => {
    const rules = stylesheet.stylesheet.rules.filter(rule => rule.type === 'rule');
    const arbitrarySizes = [];

    rules.forEach(rule => {
      rule.declarations?.forEach(decl => {
        if (decl.property === 'font-size') {
          // Allow: var(--*), rem, em, %, but flag arbitrary px values
          if (decl.value.match(/^\d+px$/) && !decl.value.match(/var\(/)) {
            arbitrarySizes.push(`${rule.selectors[0]} { font-size: ${decl.value} }`);
          }
        }
      });
    });

    expect(arbitrarySizes).toEqual([]);
  });
});

test('HTML files use CSS variable version parameter', () => {
  const htmlFiles = ['index.html', 'about.html', 'contact.html', 'case-studies/sequence-expert.html'];

  htmlFiles.forEach(file => {
    const html = fs.readFileSync(file, 'utf8');
    const cssLinkMatch = html.match(/styles\.css\?v=(\d+)/);

    expect(cssLinkMatch).toBeTruthy();
    expect(cssLinkMatch[1]).toBeTruthy(); // Version number exists
  });
});
```

**Critical for your codebase**: This directly enforces the "System First" design protocol documented in CLAUDE.md.

---

#### 7. **Mobile Responsiveness Testing**
**Why**: 60%+ traffic is mobile; responsive design is critical
**Impact**: Ensure layouts work on iPhone SE to desktop
**Tool**: [Playwright](https://playwright.dev/) for multi-viewport testing

```bash
npm install --save-dev @playwright/test
```

**What to test**:
- Layouts render correctly on mobile (375px - iPhone SE)
- Layouts render correctly on tablet (768px)
- Layouts render correctly on desktop (1440px)
- Touch targets are at least 44x44px
- No horizontal scroll on mobile
- Font sizes are readable on mobile
- Images scale properly
- Hero grid collapses to single column on mobile
- Navigation is usable on mobile

**Test file example** (`tests/responsive.spec.js`):
```javascript
const { test, expect } = require('@playwright/test');

test.describe('Mobile Responsiveness', () => {
  test('Homepage hero grid collapses on mobile', async ({ page }) => {
    await page.setViewportSize({ width: 375, height: 667 }); // iPhone SE
    await page.goto('http://localhost:8000/index.html');

    const heroGrid = page.locator('.home-hero');
    const gridColumns = await heroGrid.evaluate(el =>
      window.getComputedStyle(el).gridTemplateColumns
    );

    // Should be single column on mobile
    expect(gridColumns).not.toContain('1fr 1fr');
  });

  test('No horizontal scroll on mobile', async ({ page }) => {
    await page.setViewportSize({ width: 375, height: 667 });
    await page.goto('http://localhost:8000/index.html');

    const scrollWidth = await page.evaluate(() => document.body.scrollWidth);
    const clientWidth = await page.evaluate(() => document.body.clientWidth);

    expect(scrollWidth).toBeLessThanOrEqual(clientWidth + 1); // Allow 1px tolerance
  });

  test('Touch targets are at least 44x44px', async ({ page }) => {
    await page.setViewportSize({ width: 375, height: 667 });
    await page.goto('http://localhost:8000/index.html');

    const links = page.locator('a');
    const count = await links.count();

    for (let i = 0; i < count; i++) {
      const box = await links.nth(i).boundingBox();
      if (box) {
        expect(box.height).toBeGreaterThanOrEqual(44);
        expect(box.width).toBeGreaterThanOrEqual(44);
      }
    }
  });
});

test('Desktop layout uses two-column hero', async ({ page }) => {
  await page.setViewportSize({ width: 1440, height: 900 });
  await page.goto('http://localhost:8000/index.html');

  const heroGrid = page.locator('.home-hero');
  const gridColumns = await heroGrid.evaluate(el =>
    window.getComputedStyle(el).gridTemplateColumns
  );

  // Should be two columns on desktop
  expect(gridColumns).toContain('1fr 1fr');
});
```

**Critical for your codebase**: CLAUDE.md documents recent mobile fixes for iPhone 16 - this would prevent regressions.

---

### Priority 3: Advanced Tests (High Value, High Effort)

#### 8. **Visual Regression Testing**
**Why**: Catch unintended layout/styling changes
**Impact**: Prevent "looks fine in code, broken in browser" issues
**Tool**: [Percy](https://percy.io/) or [BackstopJS](https://github.com/garris/BackstopJS)

**What to test**:
- Screenshot comparisons of all pages (desktop + mobile)
- Hover states on buttons/links
- Focus states on interactive elements
- Card layouts and shadows
- Typography rendering (font weights, sizes, line heights)

**Test file example** (BackstopJS config):
```json
{
  "scenarios": [
    {
      "label": "Homepage - Desktop",
      "url": "http://localhost:8000/index.html",
      "viewports": [{ "width": 1440, "height": 900 }]
    },
    {
      "label": "Homepage - Mobile",
      "url": "http://localhost:8000/index.html",
      "viewports": [{ "width": 375, "height": 667 }]
    },
    {
      "label": "Case Study - Desktop",
      "url": "http://localhost:8000/case-studies/sequence-expert.html",
      "viewports": [{ "width": 1440, "height": 900 }]
    }
  ]
}
```

**When to run**: On every CSS change or before deployment

---

#### 9. **Performance Testing**
**Why**: Page speed affects SEO ranking and user experience
**Impact**: Ensure sub-3s load times, good Core Web Vitals
**Tool**: [Lighthouse CI](https://github.com/GoogleChrome/lighthouse-ci)

```bash
npm install --save-dev @lhci/cli
```

**What to test**:
- Lighthouse Performance score ≥ 90
- First Contentful Paint < 1.8s
- Largest Contentful Paint < 2.5s
- Cumulative Layout Shift < 0.1
- Total page weight < 1MB
- Image optimization (WebP format, correct sizes)
- CSS file size

**Test configuration** (`.lighthouserc.json`):
```json
{
  "ci": {
    "collect": {
      "url": ["http://localhost:8000/index.html"],
      "numberOfRuns": 3
    },
    "assert": {
      "assertions": {
        "categories:performance": ["error", {"minScore": 0.9}],
        "categories:accessibility": ["error", {"minScore": 0.9}],
        "categories:seo": ["error", {"minScore": 0.9}],
        "first-contentful-paint": ["warn", {"maxNumericValue": 1800}],
        "largest-contentful-paint": ["error", {"maxNumericValue": 2500}],
        "cumulative-layout-shift": ["error", {"maxNumericValue": 0.1}]
      }
    }
  }
}
```

**Critical for your codebase**: CLAUDE.md mentions image optimization - this would verify it's working.

---

#### 10. **Configuration & Deployment Testing**
**Why**: Netlify config mistakes can break security or caching
**Impact**: Ensure headers, redirects, and caching work as expected
**Tool**: Custom integration tests

**What to test**:
- Security headers are present (CSP, X-Frame-Options, etc.)
- Cache-Control headers match `netlify.toml` configuration
- `/portfolio` redirects to `/case-studies/sequence-expert.html` (301)
- `_redirects` rules hide `/CLAUDE.md`, `/todo/*` (404)
- HSTS header is present
- Robots.txt allows crawlers
- Sitemap.xml is accessible

**Test file example** (`tests/deployment.test.js`):
```javascript
const axios = require('axios');

describe('Netlify Configuration', () => {
  const baseURL = process.env.DEPLOY_URL || 'https://saurinchoksi.com';

  test('Security headers are present', async () => {
    const response = await axios.get(baseURL);
    expect(response.headers['x-frame-options']).toBe('DENY');
    expect(response.headers['x-content-type-options']).toBe('nosniff');
    expect(response.headers['referrer-policy']).toBe('strict-origin-when-cross-origin');
    expect(response.headers['content-security-policy']).toContain("default-src 'self'");
  });

  test('/portfolio redirects to case study (301)', async () => {
    try {
      await axios.get(`${baseURL}/portfolio`, { maxRedirects: 0 });
    } catch (error) {
      expect(error.response.status).toBe(301);
      expect(error.response.headers.location).toContain('/case-studies/sequence-expert.html');
    }
  });

  test('CLAUDE.md is not accessible (404)', async () => {
    try {
      await axios.get(`${baseURL}/CLAUDE.md`);
      fail('CLAUDE.md should return 404');
    } catch (error) {
      expect(error.response.status).toBe(404);
    }
  });

  test('CSS has cache busting version parameter', async () => {
    const response = await axios.get(baseURL);
    expect(response.data).toMatch(/styles\.css\?v=\d+/);
  });
});
```

**Critical for your codebase**: Recent commits show cache busting issues - this would catch them.

---

## Recommended Test Infrastructure Setup

### Minimal Setup (Start Here)

```bash
# Initialize npm
npm init -y

# Install core testing tools
npm install --save-dev jest

# Install validation tools
npm install --save-dev html-validate stylelint stylelint-config-standard

# Install testing utilities
npm install --save-dev cheerio glob

# Add test scripts to package.json
```

**`package.json` scripts**:
```json
{
  "scripts": {
    "test": "jest",
    "test:html": "html-validate '**/*.html' --ignore 'node_modules/**' --ignore 'todo/**'",
    "test:css": "stylelint 'assets/**/*.css'",
    "test:links": "jest tests/links.test.js",
    "test:a11y": "jest tests/accessibility.test.js",
    "test:all": "npm run test:html && npm run test:css && npm test",
    "serve": "python3 -m http.server 8000"
  }
}
```

### Full Setup (Recommended for Production)

Add all tools from Priority 1-3, plus CI/CD integration:

**GitHub Actions workflow** (`.github/workflows/test.yml`):
```yaml
name: Test Portfolio

on:
  push:
    branches: [main, claude/**]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Run HTML validation
        run: npm run test:html

      - name: Run CSS validation
        run: npm run test:css

      - name: Start local server
        run: npm run serve &

      - name: Wait for server
        run: npx wait-on http://localhost:8000

      - name: Run accessibility tests
        run: npm run test:a11y

      - name: Run link tests
        run: npm run test:links

      - name: Run SEO tests
        run: npm run test:seo

      - name: Run Lighthouse CI
        run: npx lhci autorun
```

---

## Test Coverage Gaps by Category

| Category | Current Coverage | Recommended Coverage | Priority | Effort |
|----------|------------------|----------------------|----------|--------|
| **HTML Validation** | 0% | 100% | High | Low |
| **CSS Validation** | 0% | 100% | High | Low |
| **Link Checking** | 0% | 100% | High | Low |
| **Accessibility** | 0% | 100% | High | Medium |
| **SEO Validation** | 0% | 100% | Medium | Medium |
| **Design System Compliance** | 0% | 100% | Medium | Medium |
| **Mobile Responsiveness** | 0% | 100% | High | Medium |
| **Visual Regression** | 0% | 90% | Medium | High |
| **Performance** | 0% | 100% | Medium | Medium |
| **Security Headers** | 0% | 100% | Low | Low |

---

## Cost-Benefit Analysis

### High ROI Tests (Implement First)
1. **HTML Validation** - 5 min setup, catches 80% of markup bugs
2. **Link Checking** - 10 min setup, prevents embarrassing 404s
3. **CSS Validation** - 5 min setup, enforces design system
4. **Accessibility** - 30 min setup, legal compliance + better UX

### Medium ROI Tests
5. **SEO Validation** - 1 hour setup, ensures Google indexing
6. **Mobile Responsiveness** - 2 hours setup, critical for UX
7. **Design System Compliance** - 1 hour setup, prevents technical debt

### Lower ROI Tests (Nice to Have)
8. **Visual Regression** - 3 hours setup, prevents subtle UI bugs
9. **Performance** - 2 hours setup, monitors page speed
10. **Deployment Config** - 1 hour setup, validates Netlify setup

---

## Implementation Roadmap

### Phase 1: Foundation (Week 1)
- [ ] Initialize npm and Jest
- [ ] Implement HTML validation
- [ ] Implement CSS validation
- [ ] Implement link checking
- [ ] Set up test scripts in `package.json`

### Phase 2: Quality (Week 2)
- [ ] Implement accessibility testing
- [ ] Implement SEO validation
- [ ] Implement design system compliance tests
- [ ] Add pre-commit hooks (Husky + lint-staged)

### Phase 3: Advanced (Week 3)
- [ ] Implement mobile responsiveness tests (Playwright)
- [ ] Set up visual regression testing
- [ ] Configure Lighthouse CI
- [ ] Set up GitHub Actions workflow

### Phase 4: Integration (Week 4)
- [ ] Integrate tests into Netlify deploy previews
- [ ] Add test status badges to README
- [ ] Document testing workflow in CLAUDE.md
- [ ] Train team on running/writing tests

---

## Critical Issues to Address

### 1. **No Validation of Design System**
**Risk**: Developers might hardcode `#6A3843` instead of `var(--maroon)`
**Impact**: Brand inconsistency, design debt
**Solution**: CSS linting to enforce CSS variable usage

### 2. **No Accessibility Audits**
**Risk**: WCAG violations could cause legal issues
**Impact**: ADA lawsuits, poor UX for disabled users
**Solution**: Automated Pa11y tests on every page

### 3. **No Mobile Regression Detection**
**Risk**: CSS changes break iPhone layouts
**Impact**: CLAUDE.md documents recent mobile fixes - could regress
**Solution**: Playwright tests at 375px, 768px, 1440px viewports

### 4. **No Cache Busting Verification**
**Risk**: Forgetting to increment `?v=2` breaks CSS updates
**Impact**: Recent commits show cache issues
**Solution**: Test that all HTML files reference CSS with version param

### 5. **No Broken Link Detection**
**Risk**: Rename a file, forget to update links
**Impact**: User hits 404
**Solution**: Automated link checker in CI/CD

---

## Quick Wins (Start Today)

### 1. HTML Validation (5 minutes)
```bash
npm install -g html-validate
html-validate index.html about.html contact.html case-studies/sequence-expert.html
```

### 2. CSS Validation (5 minutes)
```bash
npm install -g stylelint stylelint-config-standard
echo '{"extends": "stylelint-config-standard"}' > .stylelintrc.json
stylelint assets/styles.css
```

### 3. Link Checking (10 minutes)
```bash
npm install -g broken-link-checker
python3 -m http.server 8000 &
blc http://localhost:8000 -ro
```

### 4. Accessibility Scan (10 minutes)
```bash
npm install -g pa11y
pa11y http://localhost:8000/index.html
```

---

## Maintenance Strategy

### When to Run Tests
- **On every commit**: HTML/CSS validation, link checking
- **Before deployment**: Full test suite (a11y, SEO, performance)
- **After CSS changes**: Visual regression tests
- **Monthly**: Full accessibility audit
- **After adding new page**: Update sitemap test, run full suite

### Test Maintenance
- **Update snapshots** when intentional design changes occur
- **Review failing tests** - don't just skip them
- **Add new tests** when bugs are found (prevent regression)
- **Keep dependencies updated** - `npm outdated` monthly

---

## Success Metrics

After implementing tests, measure:
- **Zero HTML validation errors** on all pages
- **Zero broken links** on production site
- **100% WCAG AA compliance** on all pages
- **Lighthouse scores ≥ 90** for performance, accessibility, SEO
- **Zero hardcoded colors/sizes** outside design system
- **95%+ test pass rate** on every deployment

---

## Next Steps

1. **Review this document** with team
2. **Prioritize which tests to implement** based on ROI
3. **Allocate 1-2 hours** for initial test setup
4. **Start with Quick Wins** (HTML/CSS validation)
5. **Expand gradually** to full test suite
6. **Integrate into CI/CD** via GitHub Actions
7. **Document testing workflow** in CLAUDE.md

---

## Questions to Consider

- **Do you plan to add more case studies?** → SEO and sitemap tests are critical
- **Is accessibility compliance mandatory?** → Prioritize Pa11y testing
- **Do you frequently update CSS?** → Design system tests prevent regressions
- **Do you have a staging environment?** → Run full test suite on staging before production
- **What's your deployment frequency?** → Higher frequency = more need for automated tests

---

## Conclusion

While a static portfolio site can function without tests, implementing strategic testing provides:
- **Confidence** in deployments (no "did I break something?" anxiety)
- **Consistency** in design system usage
- **Compliance** with accessibility and SEO standards
- **Prevention** of regressions when adding new content
- **Professionalism** - tests signal code quality

**Recommended first steps**: Implement Priority 1 tests (HTML validation, CSS validation, link checking, accessibility) - these provide the highest ROI with minimal setup time (~1-2 hours total).
