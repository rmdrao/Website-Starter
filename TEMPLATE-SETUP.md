# Template Setup Guide

This document contains instructions for setting up this Astro template for your own use.

---

## ✅ Already Completed

- Configured for English-only i18n (French locale removed)
- Package audit: 0 security vulnerabilities
- Packages are up-to-date (Astro 5, React 19, Tailwind v4)

---

## 🎯 Remove Cosmic Themes Branding

Follow these steps to remove all Cosmic Themes branding and make this a generic template.

### 1. Package Metadata
**File:** `package.json`

```json
"name": "cosmic-themes-starter" → "your-template-name"
```

### 2. Site Data Configuration
**File:** `src/config/en/siteData.json.ts`

Replace the following values:
```typescript
name: "The Starter" → "Your Site Name"
title: "The Starter - The kitchen sink..." → "Your Custom Title"
description: "Create an amazing website..." → "Your site description"

author: {
  name: "Cosmic Themes" → "Your Name"
  email: "creator@cosmicthemes.com" → "your-email@example.com"
  twitter: "Cosmic_Themes" → "YourTwitterHandle"
}

defaultImage: {
  src: "/images/cosmic-themes-logo.jpg" → "/images/default-og-image.jpg"
  alt: "Cosmic Themes logo" → "Site logo"
}
```

### 3. Testimonials
**File:** `src/config/en/testimonialData.json.ts`

**Recommended:** Remove all current testimonials (they reference Cosmic Themes) and replace with generic placeholders or remove the testimonial section entirely.

### 4. Keystatic CMS
**File:** `keystatic.config.tsx`

Update the CMS branding:
```tsx
ui: {
  brand: { name: "Cosmic Themes" } → { name: "Admin" }
}

cloud: { project: "cosmic-themes/starter" }
→ Update to your Keystatic Cloud project or remove for local-only
```

### 5. Astro Configuration
**File:** `astro.config.mjs`

Update the site URL:
```javascript
site: "https://starter.cosmicthemes.com" → "https://yourdomain.com"
```

### 6. README
**File:** `README.md`

Remove or replace:
- Links to `cosmicthemes.com/docs/*`
- Support email: `support@cosmicthemes.com`
- License links to Cosmic Themes
- References to "Kitchen Sink Starter"

Keep the technical setup instructions (npm commands, general Astro info).

### 7. Logo & Images
**File:** `public/images/cosmic-themes-logo.png`

Delete this file or replace with your own logo. Update references in:
- `src/config/en/siteData.json.ts`

### 8. Documentation
**File:** `CLAUDE.md` (line 5)

Update the project description:
```markdown
"This is a Cosmic Themes 'Kitchen Sink' starter..."
→
"This is a comprehensive Astro-based website template..."
```

---

## 📋 Quick Checklist

- [ ] Update `package.json` name
- [ ] Update `src/config/en/siteData.json.ts` (name, author, email, twitter, defaultImage)
- [ ] Clean up `src/config/en/testimonialData.json.ts`
- [ ] Update `keystatic.config.tsx` (brand name, cloud project)
- [ ] Update `astro.config.mjs` (site URL)
- [ ] Clean up `README.md` (remove Cosmic Themes links)
- [ ] Update `CLAUDE.md` description
- [ ] Replace or delete `public/images/cosmic-themes-logo.png`
- [ ] Test: `npm run build`
- [ ] Test: `npm run dev`

---

## 🧹 Additional Cleanup (Optional)

After removing branding, consider:

### Content Cleanup
- Remove example blog posts in `src/data/blog/en/`
- Remove example authors in `src/data/authors/`
- Remove example services, projects, careers
- Create minimal placeholder content

### Remove Showcase Pages
- Delete `src/pages/examples/` directory (landing pages, section demos)
- Update navigation in `src/config/en/navData.json.ts`

### Keystatic Decision
- Keep it: Good for content-managed sites
- Remove it: Run `npm run remove-keystatic` for simpler sites

### Styling
- Update primary colors in `src/styles/tailwind-theme.css`
- Customize fonts if desired

---

## 🔄 Recommended Order

1. **Branding removal** (this document)
2. **Content cleanup** (if desired)
3. **Final package update**: `npm update` or `npx npm-check-updates -u && npm install`
4. **Test everything**: `npm run build && npm run dev`
5. **Commit as template baseline**

---

## 📝 Notes

- Make these changes BEFORE updating packages (easier troubleshooting)
- Test thoroughly after each major change
- Consider creating a git branch before starting
- Keep the technical infrastructure intact
- Maintain architectural patterns that make this template valuable

---

## 🚀 After Setup

Once your template is ready:
1. Create a new repository or save as template
2. Document any custom conventions in this file
3. Add project-specific setup instructions
4. You're ready to clone for new projects!
