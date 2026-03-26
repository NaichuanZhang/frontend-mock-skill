---
name: frontend-mock-skill
description: Create complete, high-fidelity frontend mocks with realistic data, beautiful UI, and clear mock markers. Use this skill when the user wants to prototype, mock up, or create a demo version of a frontend application. Triggers on requests involving UI mockups, frontend prototypes, demo apps, fake/placeholder data for frontends, or any request to make a project "look real" before the backend is ready. Also use when the user mentions mocking, stubbing UI, creating a demo, or building a clickable prototype.
---

# Frontend Mock Skill

Create complete, production-quality frontend mocks that look and feel real. The goal is to produce a fully functional UI with realistic data that can be viewed in the browser — suitable for stakeholder demos, design validation, and frontend development before the backend exists.

Every mock is served on **port 7777** and comes with a `remove-mock-guide.md` that documents exactly what needs to be replaced with real data when it's time to go to production.

## Step 1: Analyze the Project

Before writing any code, understand what you're working with.

**If an existing project exists:**
- Read `package.json`, config files, and existing components to detect the stack
- Identify the project's purpose, domain, and visual identity from existing code, README, or user description
- Determine what pages/components already exist vs. what needs mocking
- Look for an existing design system (CSS variables, theme files, component library)

**If starting from scratch:**
- Default to **Next.js** (App Router) unless the user specifies otherwise
- Scaffold with `npx create-next-app@latest` using TypeScript and Tailwind CSS
- Ask the user (or infer from context) what the project is about

**Identify the domain and theme.** This determines the entire aesthetic direction:
- A fintech dashboard has a different mood than a children's education app
- A luxury e-commerce site feels nothing like a developer tool
- Get this right — the mock should feel like it belongs to the actual product

## Step 2: Establish the Aesthetic Direction

The mock needs to look genuinely good — not like placeholder wireframes. Think of it as a high-fidelity prototype that could fool someone into thinking the product is real.

### Design Principles (adapted from frontend-design)

**Typography**: Choose fonts that match the project's personality. A legal SaaS gets something authoritative (e.g., Fraunces, Literata). A creative agency gets something expressive (e.g., Cabinet Grotesk, Clash Display). Pair a distinctive display font with a clean body font. Load from Google Fonts or Fontsource. Never default to Inter, Roboto, or system fonts.

**Color & Theme**: Build a cohesive palette using CSS variables. The palette should reflect the project's domain:
- Fintech/enterprise: deep blues, slate, gold accents — trust and authority
- Health/wellness: soft greens, warm neutrals, gentle gradients — calm and care
- Creative/agency: bold contrasts, unexpected accent colors — energy and confidence
- E-commerce: clean neutrals with a strong brand color — products should pop
- Gaming/entertainment: rich darks, neon accents, high contrast — excitement

Commit to a dominant color + accent strategy. Timid, evenly-distributed palettes look generic.

**Layout & Composition**: Use intentional whitespace, asymmetric grids where they serve the content, and clear visual hierarchy. Different page types need different layouts — a dashboard is dense and data-forward, a landing page is spacious and narrative.

**Motion & Polish**: Add subtle animations that make the UI feel alive. Page transitions, hover states, loading skeletons, staggered list reveals. Use CSS transitions for simple effects. These details are what separate "looks like a real product" from "looks like a homework assignment."

**Backgrounds & Atmosphere**: Go beyond flat solid colors. Use subtle gradients, noise textures, or mesh gradients for hero sections. Cards can have glass-morphism or soft shadows. The background treatment sets the overall mood.

### Match the Art Style to the Theme

This is critical. Before writing any component, decide on the visual personality and write it down as a comment at the top of your global CSS or layout file:

```css
/*
 * Design Direction: [e.g., "Refined minimalism with warm undertones"]
 * Domain: [e.g., "B2B SaaS - Project Management"]
 * Fonts: [e.g., "DM Sans (body) + Fraunces (display)"]
 * Palette: [e.g., "Slate-900 base, Amber-400 accent, warm grays"]
 */
```

This keeps the aesthetic coherent as you build out multiple pages.

## Step 3: Create the Mock Data Layer

Every mock needs realistic data. Fake but believable. The data layer makes or breaks the illusion.

### Data Strategy

**Inline mock data** for simple cases — define arrays/objects directly in component files or a `lib/mock-data.ts` file. This works well for:
- User profiles, team members
- Product listings
- Navigation items
- Static content blocks

**Next.js API routes** (or equivalent) for data that components would normally fetch. Create routes under `app/api/` that return realistic JSON. This works for:
- Dashboard metrics and charts
- Search results
- Paginated lists
- Any data a real API would serve

**Placeholder images**: Use services like `placehold.co` with appropriate dimensions and brand colors, or generate inline SVGs. For avatars, use `ui-avatars.com` or similar. For product images, use `picsum.photos` with deterministic seeds for consistency.

### Data Realism

Names should be diverse and culturally varied (not all "John Smith"). Dates should be recent and contextually appropriate. Numbers should be realistic for the domain (a SaaS dashboard showing 12 million users when the UI says "Startup Plan" breaks the illusion). Write actual paragraph text for content blocks, not "Lorem ipsum."

## Step 4: Mark Every Mock Element

This is non-negotiable. Every piece of mock data, every fake API route, every placeholder image must be marked with a `@mock:` tag so it can be found and replaced later.

Use the following convention in code comments:

| Tag | Use For | Example |
|-----|---------|---------|
| `@mock:data` | Fake data objects, arrays, fixtures | User lists, product catalogs, metrics |
| `@mock:api` | Mock API endpoints | Next.js API routes returning fake JSON |
| `@mock:asset` | Placeholder images, icons, media | placehold.co URLs, inline SVGs |
| `@mock:component` | Components that exist only for the mock | Demo banners, fake notification panels |
| `@mock:content` | Placeholder copy and text | Headlines, descriptions, blog posts |

### Marking Format

```tsx
// @mock:data - Replace with real user data from your auth provider
const mockUsers = [
  { id: 1, name: "Priya Sharma", email: "priya@example.com", role: "Admin" },
  { id: 2, name: "Marcus Johnson", email: "marcus@example.com", role: "Editor" },
];

// @mock:asset - Replace with actual product images from your CDN
const heroImage = "https://placehold.co/1200x600/1a1a2e/f0f0f0?text=Hero+Banner";

// @mock:api - Replace with real analytics endpoint
export async function GET() {
  return Response.json({
    totalUsers: 2847,
    activeToday: 342,
    revenue: 48750,
  });
}
```

Each `@mock:` comment should include a brief description of what real data should replace it. This description feeds directly into the removal guide.

See `references/mock-markers.md` for the complete marking convention.

## Step 5: Build the Pages

With the aesthetic direction set and mock data strategy planned, build out the actual pages. For each page:

1. Create the component structure
2. Wire up mock data (inline or from API routes)
3. Apply the design system (fonts, colors, spacing)
4. Add polish (animations, hover states, loading states)
5. Mark all mock elements with `@mock:` tags

### Common Page Types

**Dashboard**: Dense data layout. Cards with metrics, charts (use a charting library like Recharts or Chart.js), recent activity feed, sidebar navigation. The mock data should tell a coherent story — metrics should relate to each other logically.

**Landing/Marketing**: Hero section with strong typography, feature sections, testimonials, pricing cards, CTA buttons. The copy should sound like real marketing, not placeholder text.

**List/Table Views**: Sortable tables or card grids with realistic data. Include search bars (can be non-functional but should look real), filters, pagination.

**Detail/Profile Pages**: Rich content layout with images, metadata, related items. Show depth — the mock should feel like there's real data behind it.

**Forms**: Input fields with proper labels, validation states (show success and error states), multi-step flows if appropriate.

## Step 6: Generate remove-mock-guide.md

After building all pages, scan the entire codebase for `@mock:` tags and generate a comprehensive removal guide. This file lives at the project root.

### Guide Structure

```markdown
# Mock Removal Guide

> This guide lists every mocked element in the codebase. Each section corresponds
> to a `@mock:` tag category. Replace these with real data before going to production.

## Summary
- **X** mock data sources across **Y** files
- **Z** mock API endpoints
- **W** placeholder assets

## Mock Data (@mock:data)

| File | Line | Description | Replace With |
|------|------|-------------|--------------|
| `lib/mock-data.ts:12` | 12 | Mock user profiles | Real user data from auth provider |
| `components/Dashboard.tsx:45` | 45 | Revenue metrics | Real metrics from analytics API |

## Mock APIs (@mock:api)

| Route | File | Description | Replace With |
|-------|------|-------------|--------------|
| `/api/users` | `app/api/users/route.ts` | Returns mock user list | Connect to real user database |
| `/api/analytics` | `app/api/analytics/route.ts` | Returns fake metrics | Connect to analytics service |

## Mock Assets (@mock:asset)

| File | Line | Current | Replace With |
|------|------|---------|--------------|
| `components/Hero.tsx:8` | 8 | placehold.co hero image | Actual hero image from design |
| `components/Avatar.tsx:3` | 3 | ui-avatars.com URL | Real user avatar URLs |

## Mock Components (@mock:component)

| Component | File | Keep or Remove |
|-----------|------|----------------|
| `DemoBanner` | `components/DemoBanner.tsx` | Remove — only exists for demo |
| `MockNotifications` | `components/MockNotifications.tsx` | Remove — replace with real notification system |

## Mock Content (@mock:content)

| File | Line | Current Text | Replace With |
|------|------|-------------|--------------|
| `app/page.tsx:15` | 15 | "Transform your workflow..." | Real marketing copy |
| `components/About.tsx:8` | 8 | Company description paragraph | Real company description |

## Removal Checklist

- [ ] Replace all `@mock:data` entries with real data sources
- [ ] Connect `@mock:api` routes to real backend endpoints
- [ ] Upload real assets for all `@mock:asset` placeholders
- [ ] Remove `@mock:component` elements (or replace with production versions)
- [ ] Review and rewrite all `@mock:content` text
- [ ] Remove this file when all mocks are replaced
- [ ] Run a search for `@mock:` to verify nothing was missed
```

## Step 7: Start the Dev Server

Start the development server on port 7777:

```bash
# For Next.js
npm run dev -- -p 7777

# For Vite-based projects
npx vite --port 7777

# For plain HTML (using a simple server)
npx serve -p 7777
```

Confirm the server starts successfully and tell the user the URL: `http://localhost:7777`

If port 7777 is occupied, notify the user rather than silently picking another port.

## Framework Adaptation

While Next.js (App Router + TypeScript + Tailwind) is the default, adapt to whatever the project uses:

| Detected Stack | Mock API Strategy | Dev Server |
|---------------|-------------------|------------|
| Next.js | App Router API routes | `next dev -p 7777` |
| Vite + React | MSW or inline data | `vite --port 7777` |
| Vue/Nuxt | Nuxt server routes or inline | `nuxt dev --port 7777` |
| Plain HTML | Inline data + simple fetch mocks | `npx serve -p 7777` |

## Quality Checklist

Before telling the user the mock is ready, verify:

- [ ] All pages render without errors in the browser
- [ ] The aesthetic is cohesive and matches the project's domain
- [ ] Mock data looks realistic and tells a coherent story
- [ ] Every mock element has a `@mock:` tag
- [ ] `remove-mock-guide.md` exists at the project root
- [ ] The guide accurately lists all mocked elements
- [ ] Dev server is running on port 7777
- [ ] No console errors or warnings in the browser
