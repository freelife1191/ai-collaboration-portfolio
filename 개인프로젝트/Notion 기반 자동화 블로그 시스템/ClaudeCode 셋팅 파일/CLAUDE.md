# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Notion-powered automated blog built with Next.js that uses Notion as a CMS and automatically deploys to GitHub Pages every 10 minutes via GitHub Actions. The project follows the design and functionality of the reference site: https://hong-minji.github.io/works/

## Architecture

**Key Technology Stack:**
- Next.js 15 (App Router, React Server Components)
- TypeScript with strict mode
- **Tailwind CSS v3** with @tailwindcss/typography plugin (MANDATORY)
- **shadcn/ui** for UI components (MANDATORY)
- **Lucide React** for icons (MANDATORY)
- **Framer Motion** for animations (MANDATORY)
- **Zod** for schema validation (MANDATORY)
- Notion SDK (@notionhq/client)
- Vitest for unit testing
- Playwright for E2E testing (with MCP server for AI-assisted testing)
- Turbopack for fast development builds

**Static Site Generation:**
- Development mode: Standard Next.js server
- Production mode: Static export to `out/` directory
- GitHub Pages base path: `/nextjs-notion-blog`
- Environment-aware configuration in `next.config.ts`

## Development Commands

```bash
# Install dependencies
npm install

# Development server with Turbopack
npm run dev

# Production build (creates static export in out/)
npm run build

# Preview production build locally
npm run start

# Linting
npm run lint

# Testing
npm run test              # Run all unit tests (170 tests with Vitest)
npm run test -- --watch   # Watch mode (auto-run on file changes)

# E2E Testing
npm run test:e2e          # Run Playwright E2E tests (5 test files)
npm run test:e2e:ui       # Run E2E tests with UI mode (visual debugging)
npm run test:e2e:headed   # Run E2E tests with browser displayed
npm run test:e2e:debug    # Debug E2E tests (step-by-step execution)
npm run test:e2e:report   # View last E2E test report
```

## Test File Organization

**Current Test Structure:**

```
src/
├── lib/__tests__/              # Unit tests for lib utilities
│   ├── cache.test.ts           # Cache system tests (26 tests)
│   ├── request-memo.test.ts    # Request memoization tests (18 tests)
│   ├── errors.test.ts          # Error handling tests (47 tests)
│   ├── validation.test.ts      # Validation logic tests (49 tests)
│   └── cache-deduplication.test.ts  # Cache dedup tests (4 tests)
└── services/
    └── notion/__tests__/       # Unit tests for Notion services
        └── client.test.ts      # Notion client tests (26 tests)

tests/
├── e2e/                        # E2E tests (Playwright)
│   ├── homepage.spec.ts        # Homepage flow tests
│   ├── post.spec.ts            # Post page tests
│   ├── performance.spec.ts     # Performance tests
│   ├── youtube-mention-icon-size.spec.ts  # UI component tests
│   └── inspect-page.spec.ts    # Page inspection tests
├── debug/                      # Debug scripts (if needed)
├── fixtures/                   # Test fixtures and mock data
└── helpers/                    # Test helper functions
```

**Test Statistics:**
- ✅ **Unit Tests**: 6 files, 170 test cases (Vitest)
- ✅ **E2E Tests**: 5 files (Playwright)

**Prohibited Patterns in Root Directory:**
- ❌ `check-*.{mjs,js}` - API check scripts
- ❌ `debug-*.{mjs,js,md}` - Debug scripts and notes
- ❌ `test-*.{mjs,js,cjs,html}` - Test scripts
- ❌ `find-*.{mjs,js}` - Search/query scripts
- ❌ `query-*.{mjs,js}` - Database query scripts
- ❌ `search-*.{mjs,js}` - Search scripts
- ❌ `update-*.{mjs,js}` - Update scripts

**When Creating Test Files:**
1. **Unit tests**: Place in `__tests__/` directory next to the code being tested
   - Naming: `*.test.ts` (e.g., `cache.test.ts`)
   - Vitest will automatically discover these files
2. **E2E tests**: Place in `tests/e2e/` directory
   - Naming: `*.spec.ts` (e.g., `homepage.spec.ts`)
3. **Debug scripts**: Place in `tests/debug/` directory
4. **Test helpers**: Place in `tests/helpers/` directory

**Rationale:**
- **Unit tests near source**: Easier to maintain and find related tests
- **E2E tests centralized**: Better organization for integration tests
- **Root directory clean**: Prevents clutter and accidental commits
- **Type-safe testing**: All tests use TypeScript with full type checking

## Code Architecture

**Service Layer (`src/services/`):**
- `notion/client.ts` - Notion API integration with retry logic and error handling
- `notion/renderer.ts` - Converts Notion blocks to React components
- `render/markdown.ts` - Markdown processing utilities
- `sync/polling.ts` - Handles scheduled content synchronization

**Data Flow:**
1. Notion content (via `NotionClient`) → Cached data (`@/lib/cache.ts`)
2. Notion blocks → Rendered React components (`notionRenderer`)
3. Static export → GitHub Pages deployment

**Component Organization:**
- Route components in `src/app/` (App Router structure)
- Reusable UI components in `src/components/`
- Theme-aware components using CSS custom properties
- Dark mode via `class` strategy (see `ThemeProvider.tsx`)
- **AdUnit Component** (`src/components/AdUnit.tsx`):
  - Google AdSense ad unit component for monetization
  - Shows placeholder in development, real ads in production
  - Supports multiple ad formats (auto, rectangle, horizontal, vertical)
  - Automatically integrated in post pages (top and bottom of content)

**Key Libraries (`src/lib/`):**
- `env.ts` - Environment variable validation with typed access
- `cache.ts` - Caching layer with configurable TTL
- `toc.ts` - Table of contents generation
- `seo.ts` - SEO utilities and metadata generation
- `motion.ts` - Framer Motion animation presets

## Environment Variables

Required variables (defined in `.env.local`):
- `NOTION_API_KEY` - Notion integration API key
- `NOTION_DATABASE_ID` - ID of the Notion database containing posts

Optional variables:
- `NOTION_WEBHOOK_SECRET` - For webhook-based updates
- `GITHUB_TOKEN` - For GitHub API access
- `GITHUB_REPO` - Repository identifier
- `SENTRY_DSN` - Error tracking

Add new environment variables to `src/lib/env.ts` with proper validation.

## Notion Integration

**Database Schema:**
The Notion database must include these properties:
- `Title` (title) - Post title
- `Slug` (text) - URL-safe slug
- `Status` (select) - Post status (Publish/Draft/Hidden/Wait)
- `Date` (date) - Publication date
- `Tags` (multi-select) - Post tags
- `Label` (select) - Category label (displayed prominently on post page)
- `Description` (text) - Post description
- `CoverImage` (files) - Cover image
- `Author` (text) - Author name

**Status Options:**
- `Publish` - Published posts (visible on the blog)
- `Draft` - Draft posts (hidden from the blog)
- `Hidden` - Temporarily hidden posts
- `Wait` - Posts waiting to be published

Only posts with `Status = "Publish"` are fetched and rendered. Changing the status from "Publish" to any other value will immediately remove the post from the blog.

**Content Rendering:**
Notion block types are converted to React components in `renderer.ts`. The system supports:
- Rich text formatting (bold, italic, code, links)
- Headings, paragraphs, lists
- Code blocks with syntax highlighting (Prism.js)
- Images, files, bookmarks
- Tables, quotes, dividers

## Styling System

**IMPORTANT:** Before working on any CSS or styling tasks, you MUST read and understand `docs/TAILWIND_TYPOGRAPHY_GUIDE.md`. This guide contains essential styling conventions and patterns used throughout the project.

**Tailwind Configuration:**
- Uses CSS custom properties for theme colors (see `globals.css`)
- Dark mode: class-based strategy (`dark:` prefix)
- Typography plugin with GitHub-inspired prose styles
- Path alias: `@/*` maps to `src/*`

**Typography:**
Content from Notion is wrapped in `.prose` classes with customized styles in `tailwind.config.js`. Custom styling for code blocks, tables, blockquotes, and inline code follows GitHub's design patterns.

## CI/CD Pipeline

**GitHub Actions Workflow (`.github/workflows/gh-pages.yml`):**
1. Triggers: Push to main, manual dispatch, or 10-minute cron schedule
2. Installs dependencies
3. Builds Next.js static export with Notion data
4. Uploads artifact and deploys to GitHub Pages

**Secrets required in GitHub:**
- `NOTION_API_KEY`
- `NOTION_DATABASE_ID`

## Testing Strategy

**Unit Testing (Vitest):**
- **170 test cases** across 6 test files
- Unit tests located in `__tests__/` directories next to source code
  - `src/lib/__tests__/` - Library utilities (5 files, 144 tests)
  - `src/services/notion/__tests__/` - Notion services (1 file, 26 tests)
- Vitest configuration in `vitest.config.ts`
- Mock Notion API calls in tests (avoid live API hits)
- Focus on:
  - Cache system and deduplication (30 tests)
  - Request memoization (18 tests)
  - Error handling and retry logic (47 tests)
  - Input validation with Zod (49 tests)
  - Notion API client behavior (26 tests)
- Path resolution via `vite-tsconfig-paths` plugin
- Run with: `npm run test` or `npm run test -- --watch`

**E2E Testing (Playwright):**
- **5 test files** covering critical user flows
- E2E tests located in `tests/e2e/` with `*.spec.ts` suffix
- Playwright configuration in `playwright.config.ts`
- Tests run against development server (auto-started)
- Test coverage:
  - Homepage navigation and post listing
  - Individual post pages and content rendering
  - Performance metrics (LCP, FCP)
  - UI component sizing and layout
  - Page inspection and metadata
- Multi-browser support: Chromium, Firefox, WebKit
- Mobile device testing (Pixel 5, iPhone 12)
- Screenshots captured on failure
- Playwright MCP server configured in `.mcp.json` for AI-assisted testing
- Run with: `npm run test:e2e` or `npm run test:e2e:ui` (visual mode)

## Performance Optimization

**Static Site Generation:**
All pages are pre-rendered at build time for instant loading:
- `generateStaticParams` in `app/posts/[slug]/page.tsx` generates all post pages during build
- `dynamicParams = false` ensures only pre-generated pages are accessible
- All Notion API calls happen during build time, not at runtime

**Build-Time Caching:**
- Cache is enabled during production builds (`NODE_ENV === 'production'`)
- Cache TTL: 10 minutes during build
- Cache is disabled in development mode for immediate updates
- Reduces redundant Notion API calls during build process

**Navigation Optimization:**
- All `Link` components use `prefetch={true}` for instant navigation
- Next.js automatically prefetches linked pages when they appear in viewport
- Framer Motion animations optimized to not block click events
- Images use `pointer-events-none` to prevent drag/click interference
- `whileTap` provides immediate click feedback

**Deployment:**
- GitHub Actions builds and deploys every 10 minutes
- Static HTML files are served directly from GitHub Pages
- No server-side rendering or API calls at runtime
- Maximum performance with CDN caching

## Type Safety

- Strict TypeScript mode enabled
- All files must compile under `tsconfig.json`
- When modifying Notion schema, update types in `services/notion/client.ts`
- No `@typescript-eslint/no-explicit-any` errors enforced (configured as "off")

## Common Patterns

**Error Handling:**
- Custom `NotionApiError` class for Notion-specific errors
- Retry logic with exponential backoff for 5xx errors
- 4xx errors are not retried (client errors)

**Caching:**
- In-memory cache with TTL (see `@/lib/cache.ts`)
- Use `withCache()` wrapper for expensive operations
- Cache keys defined in `CACHE_KEYS` constant

**Component Conventions:**
- PascalCase for component files (e.g., `ProfileSidebar.tsx`)
- camelCase for utilities (e.g., `fetchPosts.ts`)
- kebab-case for route folders (e.g., `posts/[slug]/`)
- Server components by default (mark with `"use client"` if needed)

**AdSense Integration:**
- Use `AdUnit` component for all ad placements
- Ads are automatically integrated in post pages (top and bottom)
- Pass `adsensePublisherId` from settings to enable ads
- Development environment shows placeholders, production shows real ads
- Example usage:
```tsx
{adsensePublisherId && (
  <AdUnit
    publisherId={adsensePublisherId}
    adFormat="auto"
    fullWidthResponsive={true}
    label="광고"
  />
)}
```

## UI/UX Development Guidelines

**🚨 MANDATORY Technology Stack - YOU MUST USE THESE:**

| Category | Required Library | Why | Forbidden Alternatives |
|----------|-----------------|-----|----------------------|
| **UI Components** | shadcn/ui | Consistent design system | Custom components, other UI libraries |
| **Icons** | Lucide React | 1000+ icons, tree-shakable | Custom SVG, emoji, other icon libraries |
| **Animations** | Framer Motion | Declarative, performant | CSS animations, other animation libraries |
| **Validation** | Zod | Type-safe, composable | Manual validation, PropTypes, Yup, Joi |
| **Styling** | Tailwind CSS + Typography | Utility-first, consistent | Inline styles, CSS modules, styled-components |

**Before writing ANY code, ask yourself:**
- ✅ Am I using shadcn/ui for UI components?
- ✅ Am I using Lucide React for icons?
- ✅ Am I using Framer Motion for animations?
- ✅ Am I using Zod for validation?
- ✅ Am I using Tailwind CSS for styling?

**If the answer is NO to any of these, STOP and use the correct library.**

---

### **shadcn/ui Component Usage (MANDATORY):**

This project strictly follows shadcn/ui design system principles. Before implementing any UI component:

1. **ALWAYS check shadcn/ui first**: Visit https://ui.shadcn.com/docs/components to see if the component exists
2. **Use existing shadcn/ui components**: DO NOT create custom implementations for components that exist in shadcn/ui
3. **Follow shadcn/ui patterns**: Use the same Tailwind classes, CSS custom properties, and design tokens from shadcn/ui
4. **Consistency is key**: All UI components should follow shadcn/ui's design language for visual consistency

**Common shadcn/ui Components Available:**
- Layout: Card, Alert, Dialog, Sheet, Separator
- Forms: Button, Input, Label, Select, Checkbox, Radio, Switch, Textarea
- Data Display: Table, Badge, Avatar, Progress
- Feedback: Toast, Alert Dialog, Skeleton
- Navigation: Tabs, Dropdown Menu, Command
- And many more at https://ui.shadcn.com/docs/components

**When shadcn/ui component doesn't exist:**
- Use Tailwind utility classes directly
- Follow shadcn/ui's design tokens (colors, spacing, typography)
- Use CSS custom properties pattern: `hsl(var(--color-name))`
- Refer to existing shadcn/ui code for styling patterns

**Example - Card Component:**
```tsx
// ✅ CORRECT: Use shadcn/ui Card pattern
<div className="rounded-lg border bg-card text-card-foreground shadow-sm">
  <div className="p-6">Content</div>
</div>

// ❌ WRONG: Custom implementation with hardcoded colors
<div className="rounded-lg border border-gray-200 bg-white p-6">
  Content
</div>
```

**Lucide Icons Usage (MANDATORY):**

This project uses Lucide React as the exclusive icon library. Before implementing any icon:

1. **ALWAYS use Lucide icons**: Visit https://lucide.dev/icons to find the appropriate icon
2. **NO custom SVG icons**: DO NOT create custom SVG implementations for icons that exist in Lucide (1000+ icons available)
3. **Explicit imports only**: Import each icon individually for tree-shaking optimization
4. **Follow Lucide patterns**: Use Lucide's props system for consistent icon styling

**Installation:**
```bash
npm install lucide-react
```

**Usage Guidelines:**

```tsx
// ✅ CORRECT: Import and use Lucide icons
import { Camera, Heart, Settings } from 'lucide-react';

<Camera size={24} color="currentColor" strokeWidth={2} />
<Heart className="text-red-500" size={20} />
<Settings size={18} className="text-muted-foreground" />
```

```tsx
// ❌ WRONG: Custom SVG or emoji icons
<svg>...</svg>
<span>❤️</span>
```

**Available Props:**
- `size` - Icon size in pixels (default: 24)
- `color` - Icon color (default: currentColor)
- `strokeWidth` - Stroke width (default: 2)
- `className` - Tailwind classes for styling
- All standard SVG attributes (fill, stroke, etc.)

**Best Practices:**
- Use `currentColor` to inherit text color from parent
- Apply Tailwind classes via `className` for theme-aware colors
- Use semantic sizes: 16 (small), 20 (medium), 24 (default), 32 (large)
- Add ARIA attributes for accessibility when icons are interactive
- Leverage tree-shaking by importing only needed icons

**Dynamic Loading:**
For runtime icon selection, use `DynamicIcon`:
```tsx
import { DynamicIcon } from 'lucide-react/dynamic';

<DynamicIcon name="camera" size={24} />
```

**Integration with Notion Renderer (Server-Side HTML Rendering):**

**CRITICAL LIMITATION:** The Notion renderer (`renderer.ts`) generates HTML strings server-side, which are inserted using `dangerouslySetInnerHTML`. This means:
- ❌ **CANNOT** use React components (shadcn/ui components, Lucide React icons)
- ❌ **CANNOT** use client-side state or interactivity
- ✅ **CAN** use inline Tailwind classes that match shadcn/ui patterns
- ✅ **CAN** use Lucide icon SVG paths directly (copy from Lucide source)

**Best Practices for Server-Side Rendering:**
1. Use Tailwind utility classes that match shadcn/ui design tokens
2. For icons, copy the exact SVG `<path>` from Lucide React icon source code
3. Add comments documenting which Lucide icon the SVG path represents
4. Follow shadcn/ui color scheme using CSS custom properties

**Example - Using Lucide Icons in Server-Rendered HTML:**
```typescript
// ✅ CORRECT: Use Lucide's ChevronRight SVG path
// Source: https://lucide.dev/icons/chevron-right
return `<svg fill="none" stroke="currentColor" viewBox="0 0 24 24"
         stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <path d="m9 18 6-6-6-6"></path>
</svg>`;

// ❌ WRONG: Cannot use React component in HTML string
import { ChevronRight } from 'lucide-react';
return `<div>${ChevronRight}</div>`; // This won't work!
```

---

### **Framer Motion Usage (MANDATORY):**

This project uses Framer Motion for ALL animations. DO NOT use CSS animations, transitions, or other animation libraries.

**Installation:**
```bash
npm install framer-motion
```

**Usage Guidelines:**

```tsx
// ✅ CORRECT: Use Framer Motion for animations
import { motion } from 'framer-motion';

<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.3 }}
>
  Content
</motion.div>
```

```tsx
// ❌ WRONG: CSS animations or transitions
<div className="animate-fade-in transition-all duration-300">
  Content
</div>
```

**Common Animation Patterns:**

**1. Fade In:**
```tsx
<motion.div
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{ duration: 0.3 }}
>
```

**2. Slide Up:**
```tsx
<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.4, ease: "easeOut" }}
>
```

**3. Scale:**
```tsx
<motion.button
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
>
```

**4. Stagger Children:**
```tsx
<motion.ul
  variants={{
    hidden: { opacity: 0 },
    show: {
      opacity: 1,
      transition: { staggerChildren: 0.1 }
    }
  }}
  initial="hidden"
  animate="show"
>
  {items.map(item => (
    <motion.li
      key={item.id}
      variants={{
        hidden: { opacity: 0, x: -20 },
        show: { opacity: 1, x: 0 }
      }}
    >
      {item.name}
    </motion.li>
  ))}
</motion.ul>
```

**Best Practices:**
- Use `motion` wrapper for all animated elements
- Prefer `variants` for complex animations
- Use `whileHover`, `whileTap` for interactive elements
- Leverage `AnimatePresence` for exit animations
- Use `layout` prop for layout animations
- Optimize with `layoutId` for shared element transitions

**Presets Available:**
Check `src/lib/motion.ts` for reusable animation presets.

---

### **Zod Schema Validation (MANDATORY):**

This project uses Zod for ALL data validation. DO NOT use manual validation, PropTypes, or other validation libraries.

**Installation:**
```bash
npm install zod
```

**Usage Guidelines:**

```tsx
// ✅ CORRECT: Use Zod for validation
import { z } from 'zod';

const UserSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email"),
  age: z.number().int().positive().optional(),
});

type User = z.infer<typeof UserSchema>;

// Validate data
const result = UserSchema.safeParse(data);
if (result.success) {
  const user: User = result.data;
} else {
  console.error(result.error.errors);
}
```

```tsx
// ❌ WRONG: Manual validation
if (!data.name || data.name === '') {
  throw new Error('Name is required');
}
if (!data.email.includes('@')) {
  throw new Error('Invalid email');
}
```

**Common Validation Patterns:**

**1. Environment Variables (see `src/lib/env.ts`):**
```typescript
import { z } from 'zod';

const envSchema = z.object({
  NOTION_API_KEY: z.string().min(1),
  NOTION_DATABASE_ID: z.string().uuid(),
  NEXT_PUBLIC_SITE_URL: z.string().url().optional(),
});

export const env = envSchema.parse(process.env);
```

**2. API Request Validation:**
```typescript
const CreatePostSchema = z.object({
  title: z.string().min(1).max(100),
  content: z.string(),
  tags: z.array(z.string()).optional(),
  publishedAt: z.date().optional(),
});

// In API route
const body = CreatePostSchema.parse(await request.json());
```

**3. Form Validation (with React Hook Form):**
```typescript
import { zodResolver } from '@hookform/resolvers/zod';
import { useForm } from 'react-hook-form';

const formSchema = z.object({
  username: z.string().min(3).max(20),
  password: z.string().min(8),
});

const form = useForm({
  resolver: zodResolver(formSchema),
});
```

**4. Notion API Response Validation:**
```typescript
const NotionPageSchema = z.object({
  id: z.string().uuid(),
  properties: z.object({
    title: z.array(z.object({
      plain_text: z.string(),
    })),
  }),
});
```

**Best Practices:**
- Define schemas at the top of files or in separate schema files
- Use `z.infer<typeof Schema>` for TypeScript types
- Use `.safeParse()` for validation that might fail
- Use `.parse()` when you want to throw on validation error
- Combine schemas with `.merge()`, `.extend()`, `.pick()`, `.omit()`
- Use `.refine()` for custom validation logic
- Document validation rules with error messages

**When to Use Zod:**
- ✅ Environment variable validation
- ✅ API request/response validation
- ✅ Form validation
- ✅ External data validation (Notion API, third-party APIs)
- ✅ Configuration validation
- ✅ User input validation

---

## Key Files to Understand

1. `next.config.ts` - Environment-aware static export configuration
2. `src/services/notion/client.ts` - Notion API integration and data fetching
3. `src/services/notion/renderer.ts` - Block-to-component conversion
4. `src/app/page.tsx` - Homepage with post list and pagination
5. `src/app/posts/[slug]/page.tsx` - Dynamic post detail pages
6. `tailwind.config.js` - Theme and typography customization
7. `.github/workflows/gh-pages.yml` - Automated deployment pipeline
