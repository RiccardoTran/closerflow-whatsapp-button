# CloserFlow Rebranding Checklist

## Overview
This document tracks the migration from Setter AI branding to CloserFlow branding for the WhatsApp widget.

## Required Information
Before starting, gather these details:
- [ ] CloserFlow website URL (to replace `https://www.trysetter.com`)
- [ ] CDN hosting strategy (GitHub + jsDelivr, or custom CDN?)
- [ ] GitHub repository name (if using jsDelivr)
- [ ] CloserFlow logo/brand assets for examples

## Files to Update

### 1. embed.js
**Changes needed:**
- [ ] Line 10: Change ARIA label
  - FROM: `'Powered by Setter AI'`
  - TO: `'Powered by CloserFlow'`

- [ ] Line 13: Update backlink URL
  - FROM: `'https://www.trysetter.com'`
  - TO: `'https://www.closerflow.com'` (or your URL)

- [ ] Line 14: Update backlink text
  - FROM: `'Setter AI - WhatsApp Chat Widget for Business'`
  - TO: `'CloserFlow - WhatsApp Chat Widget for Business'`

- [ ] Line 24: Update Schema.org name
  - FROM: `"name": "Setter AI WhatsApp Chat Widget"`
  - TO: `"name": "CloserFlow WhatsApp Chat Widget"`

- [ ] Line 26: Update Schema.org description
  - FROM: `"description": "WhatsApp Chat Widget for Business by Setter AI"`
  - TO: `"description": "WhatsApp Chat Widget for Business by CloserFlow"`

- [ ] Line 30: Update Schema.org offer URL
  - FROM: `"url": "https://www.trysetter.com"`
  - TO: Your CloserFlow URL

- [ ] Line 34: Update Schema.org provider name
  - FROM: `"name": "Setter AI"`
  - TO: `"name": "CloserFlow"`

- [ ] Line 35: Update Schema.org provider URL
  - FROM: `"url": "https://www.trysetter.com"`
  - TO: Your CloserFlow URL

- [ ] Line 53: Update default baseUrl
  - FROM: `baseUrl: 'https://cdn.jsdelivr.net/gh/trysetter/button-generator-0001@main'`
  - TO: `baseUrl: 'https://cdn.jsdelivr.net/gh/YOUR_USERNAME/YOUR_REPO@main'`
  - OR: `baseUrl: 'https://cdn.closerflow.com/whatsapp-widget/v1'` (if using custom CDN)

### 2. widget.js
**Changes needed:**
- [ ] Line 192: Rename CSS class
  - FROM: `.mv-setter-link {`
  - TO: `.mv-closerflow-link {`

- [ ] Line 198: Rename CSS hover class
  - FROM: `.mv-setter-link:hover {`
  - TO: `.mv-closerflow-link:hover {`

- [ ] Line 280: Update footer branding
  - FROM: `⚡ Powered by <a href="https://www.trysetter.com" target="_blank" class="mv-setter-link">Setter AI</a>`
  - TO: `⚡ Powered by <a href="https://www.closerflow.com" target="_blank" class="mv-closerflow-link">CloserFlow</a>`

### 3. README.md
**Changes needed:**
- [ ] Line 1: Update header/attribution
  - FROM: `By Setter AI - the worlds fastest AI Appointment setter: https://www.trysetter.com/`
  - TO: Your CloserFlow branding message

- [ ] Line 18: Update embed script URL
  - FROM: `<script src="https://cdn.setter.ai/whatsapp-widget/v1/embed.js" async></script>`
  - TO: Your CDN URL (e.g., `https://cdn.jsdelivr.net/gh/YOUR_USERNAME/YOUR_REPO@main/embed.js`)

- [ ] Update all example code snippets with CloserFlow branding

### 4. test.html
**Changes needed:**
- [ ] Line 45: Update example baseUrl
  - FROM: `baseUrl: 'https://cdn.jsdelivr.net/gh/trysetter/button-generator-0001@main'`
  - TO: Your GitHub/CDN URL

- [ ] Line 48: Update embed script example
  - FROM: `https://cdn.jsdelivr.net/gh/trysetter/button-generator-0001@main/embed.js`
  - TO: Your CDN URL

- [ ] Lines 60-70: Update example configuration (optional)
  - Consider using CloserFlow branding in the demo (logo, business name, etc.)

### 5. CLAUDE.md
**Changes needed:**
- [ ] Line 7: Update project overview
  - FROM: `branded as "Setter AI - WhatsApp Chat Widget for Business" (trysetter.com)`
  - TO: `branded as "CloserFlow - WhatsApp Chat Widget for Business"`

- [ ] Line 16: Update SEO elements description
  - FROM: `Injects SEO elements (backlink and schema.org markup) for Setter AI branding`
  - TO: `Injects SEO elements (backlink and schema.org markup) for CloserFlow branding`

- [ ] Line 18: Update default baseUrl documentation
  - FROM: `https://cdn.jsdelivr.net/gh/trysetter/button-generator-0001@main`
  - TO: Your CDN URL

## Optional: Clean Up Deprecated Files
The `.depr/` folder contains old versions with Setter AI branding. Options:
- [ ] Delete the entire `.depr/` folder if not needed
- [ ] Update branding in deprecated files if you want to keep them
- [ ] Leave as-is if they're just historical reference

## Git Repository Setup

### Update Git Remote
```bash
# Remove connection to Setter AI repo
git remote remove origin

# Add your new CloserFlow repo
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git

# Push to your new repo
git branch -M main
git push -u origin main
```

### Repository Name Suggestions
- `closerflow-whatsapp-widget`
- `whatsapp-chat-widget`
- `closerflow-chat-button`

## CDN Hosting Options

### Option 1: GitHub + jsDelivr (Free, Recommended)
1. Create a new public GitHub repository
2. Push your rebranded code
3. Use URL format: `https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/embed.js`
4. Tag releases for version control: `@v1.0.0` instead of `@main`

**Pros:** Free, automatic CDN, easy updates
**Cons:** Public code, requires GitHub account

### Option 2: Custom CDN Domain
1. Set up subdomain: `cdn.closerflow.com`
2. Configure web server (Cloudflare, AWS CloudFront, etc.)
3. Upload `embed.js` and `widget.js` to CDN
4. Set proper CORS headers

**Pros:** Professional, private code, full control
**Cons:** Costs money, requires infrastructure setup

### Option 3: Self-Hosted
1. Host files on your main domain: `closerflow.com/widget/embed.js`
2. Ensure CORS headers allow embedding on customer sites
3. Set up caching headers

**Pros:** Simple, no external dependencies
**Cons:** Customer sites load from your main domain

## Testing Checklist
After rebranding:
- [ ] Test local server: `node server.js` → visit http://localhost:3001/
- [ ] Verify all links point to CloserFlow
- [ ] Check Schema.org markup is valid (use Google's Rich Results Test)
- [ ] Test widget loads correctly when embedded on external site
- [ ] Verify "Powered by CloserFlow" links work correctly
- [ ] Test on mobile and desktop
- [ ] Verify WhatsApp links generate correctly

## Deployment Steps
1. [ ] Complete all file changes above
2. [ ] Test locally
3. [ ] Create new GitHub repository (if using jsDelivr)
4. [ ] Update git remote and push code
5. [ ] Create a release tag (e.g., `v1.0.0`)
6. [ ] Update documentation with final CDN URLs
7. [ ] Test widget loading from CDN
8. [ ] Update any existing installations

## Notes
- Keep the `mv-` prefix on CSS classes (stands for "Mindvalley" from original, but doesn't need changing)
- The widget uses Shadow DOM, so CSS changes won't affect customer sites
- Phone number format in config: international format without `+` (e.g., `12184273128`)

## Current Status
- [ ] Planning phase - gathering information
- [ ] Rebranding in progress
- [ ] Testing
- [ ] Deployed to production

---

**Last Updated:** 2025-10-10
**Started By:** RiccardoTran
**Original Source:** https://github.com/trysetter/button-generator-0001
