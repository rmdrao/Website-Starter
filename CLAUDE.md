# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Cosmic Themes "Kitchen Sink" starter - a comprehensive Astro-based website template with landing pages and sections for SaaS, Portfolio, Services, and Blog websites. It includes built-in i18n support, Keystatic CMS integration, and uses Tailwind CSS v4 with Starwind UI components.

## Development Commands

| Command | Action |
|---------|--------|
| `npm install` | Install dependencies |
| `npm run dev` | Start dev server at `localhost:4321` |
| `npm run build` | Build production site to `./dist/` |
| `npm run preview` | Preview production build locally |
| `npm run lint` | Lint code with ESLint |
| `npm run format` | Format and fix code with ESLint and Prettier |
| `npm run config-i18n` | Configure site i18n setup (interactive script) |
| `npm run remove-keystatic` | Remove Keystatic CMS from project |
| `npm run astro ...` | Run Astro CLI commands |

## Architecture & Structure

### Framework Stack
- **Astro 5** - Core framework with SSR via Netlify adapter
- **React 19** - For interactive components
- **Tailwind CSS v4** - Styling with custom theme in `src/styles/tailwind-theme.css`
- **Starwind UI** - Pre-built component library (config in `starwind.config.json`)
- **Keystatic CMS** - Content management (accessible at `/keystatic`)

### Content Collections System

All content uses Astro's content collections with glob loaders. Collections are defined in `src/content.config.ts`:

- **blog** - Blog posts with frontmatter (title, description, authors reference, dates, images, categories, tags)
- **authors** - Author profiles with avatar and bio
- **services** - Service offerings with icons and images
- **careers** - Job postings
- **projects** - Portfolio projects with tech stack and features
- **resume** - Structured resume data (JSON format)
- **otherPages** - Legal pages and other static content
- **codeToggles** - Multi-language code examples

Content files are located in `src/data/{collection}/{locale}/{slug}/` (e.g., `src/data/blog/en/example-post/index.mdx`).

### Internationalization (i18n)

The site has a comprehensive i18n system supporting multiple locales:

**Configuration:**
- **IMPORTANT:** Run `npm run config-i18n` first to configure site i18n setup
- Locales defined in `astro.config.mjs` i18n section (currently: `en`, `fr`)
- Must match `src/config/siteSettings.json.ts` locale settings
- Default locale is `en` with no prefix; other locales use `/{locale}` prefix

**Translation System:**
- `src/config/translationData.json.ts` - Central i18n configuration:
  - `dataTranslations` - JSON data files per locale (siteData, navData, testimonialData, faqData)
  - `textTranslations` - UI text strings (use `useTranslations()` helper)
  - `routeTranslations` - Localized route names for SEO (supports wildcards like `categories/*`)
  - `localizedCollections` - Maps content collections to locale-specific routes

**Text Translation Usage:**
```ts
import { useTranslations } from "@/js/translationUtils";
const t = useTranslations("fr");
t("hero_text"); // Returns French text
```

**Content Collection Filtering:**
```ts
// Method 1: Filter any collection by language
import { filterCollectionByLanguage } from "@/js/localeUtils";
const services = await getCollection("services", ({ data }) => data.draft !== true);
const filteredServices = filterCollectionByLanguage(services, "fr");

// Method 2: Get all blog posts for a locale
import { getAllPosts } from "@/js/blogUtils";
const posts = await getAllPosts("fr"); // Gets all from src/data/blog/fr/
```

**Linking Translations with `mappingKey`:**
- Add `mappingKey` to frontmatter to link translations across locales
- Example: Both `src/data/blog/en/example-one/index.mdx` and `src/data/blog/fr/exemple-un/index.mdx` set `mappingKey: "key-one"`
- Used by language switcher and SEO hreflang generation
- Only works for collections defined in `localizedCollections`

**Content Localization:**
- Content collections use `mappingKey` in frontmatter to link translations
- Pages are organized in locale-specific directories: `src/pages/{locale}/`
- Each locale has separate content: `src/data/{collection}/{locale}/`

### Routing & Pages

Astro file-based routing with special patterns:
- `src/pages/[...page].astro` - Catch-all for dynamic blog pagination
- `src/pages/blog/[...slug].astro` - Individual blog posts
- `src/pages/categories/[category]/[...page].astro` - Category archives with pagination
- `src/pages/tags/[tag]/[...page].astro` - Tag archives with pagination
- `src/pages/fr/*` - Mirrored structure for French locale

### Layouts

Main layouts in `src/layouts/`:
- `BaseLayout.astro` - Root layout with SEO, fonts, view transitions
- `BaseHead.astro` - SEO meta tags and JSON-LD
- `BlogLayoutCenter.astro` - Centered blog post layout with table of contents
- `BlogLayoutSidebar.astro` - Blog post with sidebar layout
- `BlogIndexGrid.astro` - Blog listing grid
- `ServiceLayout.astro`, `ProjectLayout.astro`, `CareerLayout.astro` - Content type layouts

### Components

Components follow Astro conventions:
- Primarily `.astro` files (not React unless interactivity required)
- Starwind UI components imported from `@/components/` (button, accordion, card, etc.)
- Custom components organized by feature: `about/`, `blog/`, `feature/`, `hero/`, `testimonials/`, etc.
- Button component: `@/components/button/Button.astro` (use this consistently)

### Styling System

**Tailwind CSS v4 - Utility-First Approach:**
- Theme variables in `src/styles/tailwind-theme.css` using `@theme` directive
- Color system: Use `primary-*` and `base-*` variables (NOT "blue", "white", etc.)
  - `--color-primary-{50-950}` mapped to blue scale
  - `--color-base-{50-950}` mapped to neutral scale
- Starwind utilities: `background`, `foreground`, `card`, `muted`, `accent`, etc.

**Template Theme: Black & White with Primary Accents**
- Overall aesthetic uses black/white color scheme with primary color for accents
- **Primary Color Usage:**
  - For non-text accents (borders, backgrounds, underlines): Use `-primary` suffix (e.g., `border-primary`, `bg-primary`, `decoration-primary`)
  - For text accents: **Always use `text-primary-accent`** (NOT `text-primary`) - ensures proper contrast, especially in dark mode
- **Hover Effects:** Background hover states should use `hover:bg-muted` for subtle feedback

**Predefined Typography Classes** (from `src/styles/global.css`):
- Headings: Use `h1`, `h2`, `h3` classes (NOT manual font properties)
  - `<h1 class="h1">Heading 1</h1>`
  - `<h2 class="h2">Heading 2</h2>`
  - `<h3 class="h3">Heading 3</h3>`
- Descriptions: Use `description` class for text below headings
  - `<p class="description">Description text...</p>`

**CSS Files:**
- `src/styles/global.css` - Global styles, typography classes, theme customization
- `src/styles/tailwind-theme.css` - Theme configuration with CSS variables
- `src/styles/buttons.css` - Button utilities
- `src/styles/markdown-content.css` - MDX content styling
- `src/styles/fonts.css` - Font loading
- `src/styles/mos.css` - Motion-on-scroll animations

**When to Use `<style>` Tags:**
Only use direct CSS (instead of Tailwind utilities) for:
1. Complex selectors not easily targeted by Tailwind
2. Dynamic styles via CSS variables with `calc()`
3. Custom keyframe animations/transitions
4. Very specific non-utility global styles

**Conventions:**
- Use `class:list` for conditional/organized classes (not template literals)
- Prefer Tailwind utilities over custom CSS
- Use Starwind components where available

### Images

Use Astro's built-in `<Image />` component from `astro:assets`:
- Always specify `width` attribute (e.g., `width={1001}`)
- Always add `densities={[1.5, 2]}` for responsive images
- Store images in `src/assets/images/` or co-located with content in `src/data/`

### Keystatic CMS

**Configuration:** `keystatic.config.tsx`
- Local mode in dev, cloud mode in production
- Cloud project: `cosmic-themes/starter`
- Collections mirror Astro content collections but are separate per locale (e.g., `blogEN`, `blogFR`)

**Important Notes:**
- Keystatic schema must stay in sync with `src/content.config.ts` schemas
- **Authors Collection Limitation:** Due to Keystatic relationship field limitations with i18n, the `authors` collection is NOT separated by locale. If multilingual author info is needed, create separate entries with language indicators in slugs (e.g., `john-doe-en`, `john-doe-fr`)

### Scripts

Utility scripts in `scripts/`:
- `config-i18n.js` - Interactive i18n setup wizard
- `remove-keystatic.js` - Removes Keystatic integration

### Utilities

TypeScript utilities in `src/js/`:
- `blogUtils.ts` - Blog post filtering, sorting, pagination
- `localeUtils.ts` - Locale detection from URLs
- `translationUtils.ts` - Text translation helpers
- `jsonLD.ts` - Structured data generation
- `textUtils.ts` - Text manipulation helpers

## Coding Guidelines

### General Rules
- **TypeScript** - Use ES6+ syntax, imports (never `require`)
- **Astro First** - All components should be `.astro` unless React interactivity is required
- **JSDoc** - Add JSDoc comments to exported functions and classes
- **Minimal** - Prioritize minimalist, efficient code without over-engineering

### Component Patterns
- Always use Button component from `@/components/button/Button.astro`
- Import images via `astro:assets` with proper width and densities
- Use `class:list` for complex class combinations
- Ensure high accessibility (a11y) standards with ARIA roles

### Styling
- Use Tailwind CSS v4 utilities (utility-first approach)
- Reference color variables: `primary-*` and `base-*` (NOT literal colors)
- **Text accents:** Use `text-primary-accent` for primary-colored text (NOT `text-primary`)
- **Hover effects:** Use `hover:bg-muted` for background hover states
- **Typography:** Use predefined classes `h1`, `h2`, `h3`, `description` (NOT manual font properties)
- Prefer Starwind UI components when available
- Only use `class:list` when needed for readability or conditional classes

### i18n Considerations
- When creating new routes, update `routeTranslations` in `src/config/translationData.json.ts`
- Use `mappingKey` in content frontmatter to link translations
- Content collections need locale-specific variants
- Use `getLocaleFromUrl()` and `useTranslations()` helpers consistently
- Filter collections by locale: `filterCollectionByLanguage(items, locale)` or `getAllPosts(locale)` for blog
- Add new localized collections to `localizedCollections` in `translationData.json.ts`

## Special Notes

- **View Transitions:** Enabled by default (configurable in `siteSettings.json.ts`)
- **Animations:** Use `useAnimations` setting; animations configured in tailwind-theme.css
- **SEO:** Handled via BaseHead.astro and JSON-LD utilities
- **Draft Content:** Set `draft: true` in frontmatter to exclude from build
- **Image Optimization:** Handled by Astro assets (DO NOT enable compress plugin for images)
