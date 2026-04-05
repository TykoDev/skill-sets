# Design System Specification Template

## Component Specification Format

### Component Definition Template

```markdown
## Component: [ComponentName]

### Classification
- **Level**: Atom | Molecule | Organism | Template | Page
- **Category**: UI primitive | Form | Navigation | Data display | Feedback | Layout

### Description
[What this component does and when to use it]

### Props / API
| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| variant | 'primary' \| 'secondary' \| 'ghost' | 'primary' | No | Visual style |
| size | 'sm' \| 'md' \| 'lg' | 'md' | No | Size variant |
| disabled | boolean | false | No | Disabled state |
| children | ReactNode | — | Yes | Content |

### States
| State | Visual Description | Trigger |
|-------|-------------------|---------|
| Default | [Description] | Initial render |
| Hover | [Description] | Mouse over |
| Active | [Description] | Mouse down / touch |
| Focus | [Description] | Keyboard focus |
| Disabled | [Description] | disabled=true |
| Loading | [Description] | async operation |
| Error | [Description] | validation failure |

### Accessibility
- **Role**: [ARIA role if not semantic default]
- **Keyboard**: [Key interactions — Enter, Space, Escape, Tab]
- **Screen reader**: [Announcement behavior]
- **Focus management**: [Focus trap, focus restore]

### Usage Guidelines
**Do**: [Correct usage patterns]
**Don't**: [Anti-patterns to avoid]
```

---

## Design Tokens

Design tokens are the atomic values that define the visual design language.
Define tokens as CSS custom properties or a structured token file.

### Color Tokens

```css
:root {
  /* Brand */
  --color-brand-50: hsl(220, 90%, 96%);
  --color-brand-100: hsl(220, 85%, 90%);
  --color-brand-500: hsl(220, 80%, 50%);   /* Primary */
  --color-brand-600: hsl(220, 80%, 40%);   /* Primary hover */
  --color-brand-900: hsl(220, 70%, 15%);

  /* Semantic */
  --color-text-primary: hsl(220, 20%, 10%);
  --color-text-secondary: hsl(220, 10%, 45%);
  --color-text-inverse: hsl(0, 0%, 100%);
  --color-bg-primary: hsl(0, 0%, 100%);
  --color-bg-secondary: hsl(220, 20%, 97%);
  --color-border: hsl(220, 15%, 85%);

  /* Feedback */
  --color-success: hsl(145, 65%, 40%);
  --color-warning: hsl(40, 95%, 50%);
  --color-error: hsl(0, 75%, 55%);
  --color-info: hsl(210, 80%, 55%);
}
```

### Spacing Tokens

```css
:root {
  --space-1: 0.25rem;    /* 4px */
  --space-2: 0.5rem;     /* 8px */
  --space-3: 0.75rem;    /* 12px */
  --space-4: 1rem;       /* 16px */
  --space-5: 1.5rem;     /* 24px */
  --space-6: 2rem;       /* 32px */
  --space-8: 3rem;       /* 48px */
  --space-10: 4rem;      /* 64px */
  --space-12: 6rem;      /* 96px */
}
```

### Typography Tokens

```css
:root {
  /* Font families */
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, monospace;

  /* Font sizes */
  --text-xs: 0.75rem;     /* 12px */
  --text-sm: 0.875rem;    /* 14px */
  --text-base: 1rem;      /* 16px */
  --text-lg: 1.125rem;    /* 18px */
  --text-xl: 1.25rem;     /* 20px */
  --text-2xl: 1.5rem;     /* 24px */
  --text-3xl: 1.875rem;   /* 30px */
  --text-4xl: 2.25rem;    /* 36px */

  /* Line heights */
  --leading-tight: 1.25;
  --leading-normal: 1.5;
  --leading-relaxed: 1.75;

  /* Font weights */
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
}
```

### Shadow, Radius, and Motion Tokens

```css
:root {
  /* Shadows */
  --shadow-sm: 0 1px 2px hsl(0 0% 0% / 0.05);
  --shadow-md: 0 4px 6px hsl(0 0% 0% / 0.07);
  --shadow-lg: 0 10px 15px hsl(0 0% 0% / 0.1);

  /* Border radius */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 0.75rem;
  --radius-full: 9999px;

  /* Transitions */
  --duration-fast: 150ms;
  --duration-normal: 250ms;
  --duration-slow: 400ms;
  --ease-default: cubic-bezier(0.4, 0, 0.2, 1);
}
```

---

## Responsive Breakpoints

| Name | Min Width | Target Devices |
|------|----------|---------------|
| `sm` | 640px | Large phones (landscape) |
| `md` | 768px | Tablets |
| `lg` | 1024px | Small laptops |
| `xl` | 1280px | Desktops |
| `2xl` | 1536px | Large desktops |

---

## Accessibility Compliance Checklist (WCAG 2.2 Level AA)

### Perceivable
- [ ] All images have descriptive alt text (1.1.1)
- [ ] Video has captions and audio descriptions (1.2.x)
- [ ] Content is structured with semantic HTML (1.3.1)
- [ ] Color is not the only means of conveying information (1.4.1)
- [ ] Text contrast ratio meets 4.5:1 (normal) / 3:1 (large) (1.4.3)
- [ ] Text can be resized to 200% without loss of content (1.4.4)

### Operable
- [ ] All functionality is keyboard accessible (2.1.1)
- [ ] No keyboard traps (2.1.2)
- [ ] Skip navigation link provided (2.4.1)
- [ ] Pages have descriptive titles (2.4.2)
- [ ] Focus order is logical (2.4.3)
- [ ] Link purpose is clear from text (2.4.4)
- [ ] Focus indicator is visible (2.4.7)
- [ ] Touch/pointer targets are at least 24×24px (2.5.8)

### Understandable
- [ ] Page language is declared (3.1.1)
- [ ] Form inputs have labels (3.3.2)
- [ ] Error messages are descriptive and suggest corrections (3.3.3)

### Robust
- [ ] HTML validates (4.1.1)
- [ ] ARIA roles, states, and properties are correct (4.1.2)
- [ ] Status messages are announced (4.1.3)
