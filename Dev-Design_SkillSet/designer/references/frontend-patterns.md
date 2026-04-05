# Frontend Architecture Patterns Reference

## Single-Page Application (SPA)

### Description
Entire application loads once; subsequent navigation handled client-side
via JavaScript. The server serves a single HTML file; routing, rendering,
and data fetching happen in the browser.

### Advantages
- Smooth, app-like transitions between views
- Rich interactivity without page reloads
- Decoupled from backend (API-first)
- Simple deployment (static files)

### Disadvantages
- Poor SEO without additional configuration (hydration, pre-rendering)
- Slower initial load (full JS bundle)
- Accessibility challenges with client-side routing
- Memory management responsibility on the client

### Best For
Internal dashboards, admin panels, real-time collaboration tools,
applications behind authentication.

---

## Server-Side Rendering (SSR)

### Description
Pages are rendered on the server for each request. The browser receives
fully-formed HTML, then "hydrates" it with JavaScript for interactivity.

### Advantages
- Excellent SEO (crawlers see full HTML)
- Faster First Contentful Paint (HTML arrives ready)
- Better for users with slow connections or devices
- Access control enforced server-side

### Disadvantages
- Server required (not just static files)
- Increased server costs
- Hydration mismatch bugs
- Higher framework complexity

### Best For
Content-heavy sites, e-commerce, SEO-critical applications,
applications with public and authenticated sections.

---

## Static Site Generation (SSG)

### Description
All pages are pre-built at compile time. The output is static HTML, CSS,
and JavaScript files deployed to a CDN.

### Advantages
- Maximum performance (pre-built, CDN-served)
- Lowest hosting cost (static files)
- Excellent SEO
- Simple deployment and caching

### Disadvantages
- Content changes require rebuild
- Not suitable for dynamic/personalized content
- Build times grow with page count

### Best For
Documentation sites, blogs, marketing pages, portfolios.

---

## Islands Architecture (Astro)

### Description
Pages are rendered as static HTML by default. Only specific "islands"
of interactivity are hydrated with JavaScript. The rest remains static.

### Advantages
- Zero JavaScript by default — excellent performance
- Selective hydration reduces bundle size
- Multi-framework support (React + Vue + Svelte on one page)
- Perfect Core Web Vitals scores

### Disadvantages
- Limited to content-driven sites
- Complex interactive applications are awkward
- Hydration boundaries require careful planning
- Smaller ecosystem than SPA frameworks

### Best For
Content sites with selective interactivity, documentation with
interactive code examples, marketing sites with animated widgets.

---

## Micro-Frontends

### Description
Split the frontend into independently deployable units, each owned by a
different team. Units integrate at runtime via Module Federation,
Web Components, or iframe composition.

### Integration Approaches

| Approach | Isolation | Performance | Complexity |
|----------|-----------|-------------|-----------|
| **Module Federation** | Medium | Good | Medium |
| **Web Components** | High | Medium | Low |
| **iframe** | Complete | Poor | Low |
| **Server-side composition** | Medium | Good | High |

### Advantages
- Team autonomy — each team owns their micro-frontend end-to-end
- Independent deployability
- Framework agnosticism (team A uses React, team B uses Vue)
- Fault isolation (one micro-frontend crashing doesn't take down others)

### Disadvantages
- Shared dependencies management
- Consistent UX across micro-frontends
- Tooling complexity
- Cross-micro-frontend state management

### Best For
Large enterprises with multiple autonomous teams (Spotify model), platforms
with distinct product areas, gradually migrating legacy frontends.

### Anti-Pattern Warning
Do NOT use micro-frontends with a single team. The overhead is only
justified when multiple teams need independent deployment.

---

## Signals-Based Reactivity (2025-2026 Trend)

Signals provide fine-grained, synchronous reactivity without a virtual DOM
diff step. A signal holds a value; when the value changes, only the exact
DOM nodes that depend on it update — no component re-rendering.

### Framework Adoption

| Framework | Signal API | Status (2026) |
|-----------|-----------|---------------|
| **Angular** | `signal()`, `computed()`, `effect()` | Production (v17+) |
| **Vue** | `ref()`, `computed()`, `watch()` | Production (Vue 3+) |
| **Svelte** | `$state`, `$derived`, `$effect` | Production (Svelte 5) |
| **Solid.js** | `createSignal()`, `createMemo()`, `createEffect()` | Production |
| **React** | Experimental (React Forget/Compiler) | React Compiler auto-memoizes |
| **Preact** | `@preact/signals` | Production |

### Performance Impact
- Signals eliminate unnecessary re-renders
- Virtual DOM diffing replaced with targeted DOM mutations
- Smaller runtime overhead
- Better performance at scale (100s of reactive values)

---

## Server Components (React)

### Description
Components that render exclusively on the server. They can access databases,
file systems, and backend services directly. The rendered output (not the
component code) is sent to the client.

### Key Rules
1. Server Components are the default in Next.js App Router
2. Client Components must be explicitly marked with `'use client'`
3. Server Components can import Client Components (not vice versa)
4. Server Components cannot use hooks (`useState`, `useEffect`, etc.)
5. Server Components can be async — await data directly

### Mental Model
```
Server Components → Fetch data, render HTML fragments → Send to client
Client Components → Interactive UI, event handlers, browser APIs
```
