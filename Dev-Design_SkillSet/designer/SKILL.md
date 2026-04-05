---
name: designer
description: >-
  This skill should be used when the user or commander asks to "design the
  frontend architecture", "create a component system", "define the UI
  architecture", "plan state management", "specify accessibility requirements",
  "set performance targets", "choose between SPA and SSR", "design the user
  experience flow", or "create a design system specification". It produces
  frontend architecture specifications, component hierarchies, state management
  strategy, accessibility requirements, and performance targets.
version: 1.0.0
---

# Designer — Frontend Architecture & UI/UX Specification

## Purpose

This skill performs Phase 4 of the Dev Design SkillSet pipeline. It takes
the gatekeeper-design-approved architecture (from architect) and produces the
frontend-specific architecture: rendering strategy, component system,
state management, routing, accessibility compliance, and performance targets.

## When to Activate

Activate when commander delegates Phase 4 (Frontend Design) after the
architect's architecture document has been approved by gatekeeper-design.

---

## Workflow

### Step 1: Select Frontend Architecture Pattern

Based on the requirements and architecture document, select the rendering
and architecture approach. Document the decision rationale.

| Pattern | Best For | Trade-offs |
|---------|---------|-----------|
| **SPA (Client-Side Rendering)** | Internal tools, dashboards, real-time apps | No SEO, slower initial load |
| **SSR (Server-Side Rendering)** | Content sites, SEO-critical, authenticated apps | Server cost, complexity |
| **SSG (Static Site Generation)** | Blogs, documentation, marketing pages | Build-time data only |
| **ISR (Incremental Static Regen)** | E-commerce, news, large catalogs | Framework-specific (Next.js) |
| **Islands Architecture** | Content sites with selective interactivity | Astro-specific |
| **Micro-Frontends** | Multiple teams, independent deployability | Integration complexity |

Consult `references/frontend-patterns.md` for detailed pattern descriptions.

### Step 2: Select Framework and Build Tooling

Recommend a specific framework from the tech-stacks templates based on:
- Architecture pattern selected in Step 1
- Backend technology (full-stack TypeScript? Python backend?)
- Team expertise and hiring considerations
- Performance requirements
- SEO requirements

Reference the appropriate tech-stack template:
- `tech-stacks/react-nextjs.md` — SSR/ISR with Next.js
- `tech-stacks/react-tanstack.md` — Type-safe SPA/SSR with TanStack
- `tech-stacks/vue-nuxt.md` — Vue ecosystem with Nuxt
- `tech-stacks/angular.md` — Enterprise Angular
- `tech-stacks/astro.md` — Content sites with Islands
- `tech-stacks/vite-spa.md` — Client-side SPA baseline
- `tech-stacks/svelte-sveltekit.md` — Svelte ecosystem

### Step 3: Design Component System

Define the component hierarchy using atomic design principles:

```markdown
## Component Hierarchy

### Atoms (Primitives)
- Button, Input, Label, Icon, Badge, Avatar, Spinner

### Molecules (Simple Compositions)
- FormField (Label + Input + Error), SearchBar, NavigationLink

### Organisms (Complex Compositions)
- Header, Sidebar, DataTable, UserCard, LoginForm

### Templates (Page Layouts)
- DashboardLayout, AuthLayout, PublicLayout

### Pages (Route-Level)
- HomePage, DashboardPage, SettingsPage, UserProfilePage
```

Consult `references/design-system-template.md` for the complete design
system specification format.

### Step 4: Define State Management Strategy

Specify how data flows through the application:

| State Type | Storage | Tools | Examples |
|-----------|---------|-------|---------|
| **Server state** | Remote, cached locally | TanStack Query, SWR, useFetch | API data, user records |
| **Client UI state** | In-memory | Zustand, Pinia, Signals | Theme, sidebar open/closed |
| **URL state** | Browser URL | Router search params | Filters, pagination, active tab |
| **Form state** | Component-level | TanStack Form, React Hook Form | Form inputs, validation |
| **Auth state** | Cookies + context | Auth provider | User session, permissions |

**Rules**:
- Never use Redux/NgRx for server state — use TanStack Query or equivalent
- Server state and client state are separate concerns — never mix stores
- URL is state too — use router params for shareable/bookmarkable state

### Step 5: Define Routing Strategy

Specify the routing architecture:
- File-based routing conventions (page structure → URL structure)
- Dynamic route patterns (parameters, catch-all)
- Route groups and layouts (nested, parallel)
- Route guards and middleware (auth, permissions)
- Code splitting strategy (per-route, per-feature)
- Loading and error states per route

### Step 6: Specify Accessibility Requirements

Define WCAG 2.2 compliance scope:

| Requirement | WCAG Criterion | Implementation |
|------------|---------------|----------------|
| Touch target size | 2.5.8 (AA) | Minimum 24×24px for all interactive elements |
| Focus visibility | 2.4.7 (AA) | Clear focus indicator on all focusable elements |
| Color contrast | 1.4.3 (AA) | 4.5:1 for text, 3:1 for large text |
| Keyboard navigation | 2.1.1 (A) | All functionality accessible via keyboard |
| Heading hierarchy | 1.3.1 (A) | Single `<h1>` per page, logical heading order |
| Alt text | 1.1.1 (A) | All images have descriptive alt text |
| Form labels | 1.3.1 (A) | All form inputs have associated labels |
| Auth accessibility | 3.3.8 (AA) | Passkeys and password managers work |

### Step 7: Set Performance Targets

Define measurable performance goals:

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| LCP (Largest Contentful Paint) | < 2.5s | Core Web Vitals |
| INP (Interaction to Next Paint) | < 200ms | Core Web Vitals |
| CLS (Cumulative Layout Shift) | < 0.1 | Core Web Vitals |
| Total bundle size (JS) | < [X] KB | Build analysis |
| Time to Interactive | < [X] s | Lighthouse |

### Step 8: Submit to Gatekeeper

Package the complete frontend specification and submit to `gatekeeper-design`
for adversarial review.

---

## Output Format

The designer produces one consolidated deliverable:

**Frontend Architecture Specification** containing:
1. Architecture pattern decision with rationale
2. Framework and build tool selection
3. Component hierarchy and design system spec
4. State management strategy
5. Routing architecture
6. Accessibility requirements (WCAG 2.2)
7. Performance targets (Core Web Vitals)
8. Browser/device support matrix

---

## Additional Resources

### Reference Files

For detailed patterns and templates:
- **`references/frontend-patterns.md`** — Detailed frontend architecture patterns, Islands architecture, micro-frontends, and signals-based reactivity
- **`references/design-system-template.md`** — Component specification format, design tokens, and accessibility checklist
