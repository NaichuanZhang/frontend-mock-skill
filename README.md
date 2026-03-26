# Frontend Mock Skill

A Claude Code skill that creates complete, high-fidelity frontend mocks with realistic data, beautiful UI, and clear mock markers. Every mock is served on port 7777 and comes with a `remove-mock-guide.md` that documents exactly what needs to be replaced with real data.

## What It Does

- Scaffolds a full Next.js app (or adapts to your existing stack)
- Generates realistic mock data — API routes, user profiles, products, metrics
- Applies distinctive, domain-appropriate aesthetics (not generic AI slop)
- Tags every mocked element with `@mock:` comments for easy discovery
- Generates a `remove-mock-guide.md` listing every mock and how to replace it
- Serves everything on port 7777

## File Architecture

| File | Purpose | When Loaded |
|------|---------|-------------|
| `SKILL.md` | Core workflow — project analysis, design, mock data, marking, guide generation | Immediate invocation |
| `references/mock-markers.md` | Detailed `@mock:` tagging convention with examples for each category | When tagging mock elements |

### Mock Marker Categories

| Tag | Purpose |
|-----|---------|
| `@mock:data` | Fake data objects, arrays, fixtures |
| `@mock:api` | Mock API endpoints |
| `@mock:asset` | Placeholder images, icons, media |
| `@mock:component` | Components that exist only for the mock |
| `@mock:content` | Placeholder text and copy |

## Installation

### Manual Installation

```bash
mkdir -p ~/.claude/skills/frontend-mock-skill/references
cp frontend-mock-skill/SKILL.md ~/.claude/skills/frontend-mock-skill/
cp frontend-mock-skill/references/mock-markers.md ~/.claude/skills/frontend-mock-skill/references/
```

### Direct Clone

```bash
git clone https://github.com/YOUR_USERNAME/frontend-mock-skill.git /tmp/frontend-mock-skill
mkdir -p ~/.claude/skills/frontend-mock-skill
cp -r /tmp/frontend-mock-skill/frontend-mock-skill/* ~/.claude/skills/frontend-mock-skill/
```

After installation, invoke via: `/frontend-mock-skill`

## Usage

Ask Claude Code to create a frontend mock:

```
Create a frontend mock for a SaaS analytics dashboard called "Metrica"
```

```
Mock up a boutique e-commerce storefront for organic skincare products
```

```
Create a mock for a creative agency portfolio site with case studies
```

The skill will:
1. Analyze your project (or scaffold a new Next.js app)
2. Establish an aesthetic direction matched to your domain
3. Create realistic mock data and API routes
4. Build polished pages with animations and micro-interactions
5. Tag every mock element with `@mock:` comments
6. Generate `remove-mock-guide.md` at the project root
7. Start the dev server on port 7777

## Benchmark Results

Tested across 3 scenarios (SaaS dashboard, e-commerce, agency portfolio):

| Metric | With Skill | Without Skill | Delta |
|--------|-----------|---------------|-------|
| Pass Rate | 100% | 56% | +44% |
| Mock Markers | Present | Missing | -- |
| Removal Guide | Generated | Missing | -- |
| Port 7777 | Configured | Default 3000 | -- |
| Custom Fonts | Distinctive choices | Defaults (Inter, Geist) | -- |
