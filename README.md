# Saurin Choksi Portfolio

Static HTML/CSS portfolio site ready for GitHub Pages or Netlify deployment.

## File Structure

```
portfolio-site/
├── index.html              # Homepage
├── about.html              # About page
├── contact.html            # Contact page
├── case-studies/
│   └── sequence-expert.html   # Sequence Expert case study
├── assets/
│   ├── images/
│   │   └── hero-photo.jpg     # Your hero photo (add this)
│   └── logos/
│       ├── disney.svg         # Client logos (add these)
│       ├── netflix.svg
│       └── ...
└── README.md
```

## Setup Instructions

### 1. Add Your Images

Replace the placeholders with your actual images:

**Hero Photo:**
- Add your photo to `assets/images/hero-photo.jpg`
- Update `index.html` to use the image instead of the placeholder

**Client Logos:**
- Add logo images to `assets/logos/`
- Update `index.html` to reference them instead of placeholders
- Recommended: Use SVG format for crisp scaling
- Apply grayscale filter and reduced opacity in CSS (already set up)

### 2. Update Contact Info

In `contact.html`, update:
- Email address (replace `your.email@example.com`)
- LinkedIn URL (replace with your actual profile URL)

### 3. Deploy to GitHub + Netlify

**Create GitHub Repository:**
```bash
git init
git add .
git commit -m "Initial portfolio site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/portfolio.git
git push -u origin main
```

**Connect to Netlify:**
1. Go to [netlify.com](https://netlify.com) and sign up/log in
2. Click "Add new site" → "Import an existing project"
3. Connect your GitHub account
4. Select your portfolio repository
5. Deploy settings: Leave defaults (no build command needed for static HTML)
6. Click "Deploy site"

**Connect Your Domain:**
1. In Netlify: Go to Site settings → Domain management → Add custom domain
2. Enter `saurinchoksi.com`
3. Netlify will give you DNS records to add

**Update DNS in Squarespace:**
1. Log into Squarespace
2. Go to Settings → Domains → saurinchoksi.com → DNS Settings
3. Add the records Netlify provided:
   - A record: `75.2.60.5` (Netlify's load balancer)
   - CNAME: `www` → `your-site-name.netlify.app`
4. Wait 24-48 hours for DNS propagation

### 4. Verify

Once DNS propagates:
- `https://saurinchoksi.com` should show your new site
- `https://www.saurinchoksi.com` should redirect to the main domain
- HTTPS should be automatically enabled by Netlify

## Making Updates

With this setup, updating your site is simple:

1. Edit HTML/CSS files locally (or have Claude generate updates)
2. Commit and push to GitHub:
   ```bash
   git add .
   git commit -m "Update description"
   git push
   ```
3. Netlify automatically deploys within ~30 seconds

## Style Guide Reference

See `/mnt/project/saurin-choksi-style-guide.md` for the complete design system including:
- Color palette (Maroon, Gold, Dusty Rose, Cream, etc.)
- Typography (Fraunces, DM Sans)
- Spacing and component styles

## Adding New Case Studies

1. Duplicate `case-studies/sequence-expert.html`
2. Update content
3. Add a new card in `index.html` under Featured Work
4. Push to GitHub

---

Built with Claude. Designed for fast iteration.
