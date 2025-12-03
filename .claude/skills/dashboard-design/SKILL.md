# Dashboard Design Skill

Clean, minimal dashboard/app interfaces inspired by shadcn/ui, Notion, Linear, and ClickUp.

## Core Principle

**Functional over Flashy**: Dashboards are tools, not marketing. Prioritize information density, readability, and efficiency over visual drama.

## Visual Style

### Color Palette
- **Background**: White or off-white (slate-50, gray-50)
- **Borders**: Subtle grays (slate-200, gray-200)
- **Text**: Dark grays (slate-900, gray-900) with muted secondary (slate-600)
- **Accent**: Inherit from brand but use sparingly (buttons, active states, key actions)
- **Shadows**: Minimal (`shadow-sm` for cards, `shadow-md` for modals)

### Typography
- **Base size**: 14-16px (text-sm to text-base)
- **Headings**: Smaller than landing pages (text-lg to text-xl max)
- **Weight**: Regular (400) for body, medium (500) for labels, semibold (600) for headings
- **Line height**: Tight for compact density (leading-tight, leading-snug)

### Spacing
- **Compact**: Use smaller gaps (gap-2, gap-4) vs landing pages
- **Consistent**: Follow 4px/8px scale (p-2, p-4, p-6)
- **Dense tables**: Tight padding (px-3 py-2) for rows

## Layout Structure

### Sidebar Navigation
```
- Width: 240-280px, collapsible to 60px (icons only)
- Background: slate-50 or white with border-r
- Items: Icon + text, subtle hover (hover:bg-slate-100)
- Active state: bg-slate-200 or subtle accent background
- Grouped sections with muted labels (text-xs uppercase)
```

### Header
```
- Height: 56-64px
- Background: White with border-b
- Contents: Breadcrumbs (left), search (center), user menu (right)
- Sticky positioning
```

### Main Content
```
- Padding: p-6 to p-8
- Background: White or slate-50
- Max width: Full width or max-w-7xl for readability
```

### Footer (Dashboard-specific)
```
- Minimal inline footer OR simple bar at bottom of sidebar
- Essential links only (Help, Privacy, Terms)
- Small text (text-xs, text-slate-500)
- No bold design elements
```

## Component Patterns

### Cards
```html
<div class="bg-white border border-slate-200 rounded-lg p-4 shadow-sm">
  <h3 class="text-sm font-semibold text-slate-900 mb-2">Card Title</h3>
  <p class="text-sm text-slate-600">Content</p>
</div>
```

### Tables
```
- Striped or bordered, not both
- Header: bg-slate-50, font-medium, text-xs uppercase
- Rows: py-2 px-3, hover:bg-slate-50
- Text: text-sm for readability
```

### Forms
```
- Input: border-slate-300, focus:ring-2 focus:ring-[accent], rounded-md
- Label: text-sm font-medium text-slate-700 mb-1
- Helper text: text-xs text-slate-500
- Error: text-xs text-red-600
```

### Buttons
```
- Primary: bg-[accent], small padding (px-4 py-2), text-sm
- Secondary: bg-white border border-slate-300
- Ghost: hover:bg-slate-100
- Icon buttons: Square (w-9 h-9), centered icon
```

### Empty States
```
- Centered icon (w-12 h-12, text-slate-400)
- Heading: text-base font-medium
- Description: text-sm text-slate-600
- Action button: Primary style
- No decorative illustrations
```

## Distinct from Landing Pages

| Aspect | Landing Page | Dashboard |
|--------|--------------|-----------|
| Text size | Large (text-5xl heroes) | Small (text-lg max) |
| Shadows | Bold (shadow-xl) | Subtle (shadow-sm) |
| Colors | Vibrant, gradients | Muted, grays |
| Spacing | Generous (p-12) | Compact (p-4) |
| Animations | Dramatic | Minimal/functional |
| Footer | Bold, multi-column | Minimal, inline |

## Theme Consistency

- Use same **font family** as landing page
- Use same **accent color** but sparingly
- Use same **border radius** values
- Maintain **brand recognition** through color, not flashiness

## Reference: shadcn/ui Patterns

Study shadcn/ui components for:
- Muted backgrounds (slate-50)
- Consistent border colors (slate-200)
- Subtle focus states (ring-2)
- Clean hover interactions
- Compact information density

## Tailwind v4 Compatibility

- Use `gap-*` instead of `space-x-*` or `space-y-*` (removed in v4)
- Include `@config` directive if using custom config
- Avoid custom CSS resets that break Tailwind utilities

## Implementation Checklist

- Sidebar with icons and collapsible state
- Compact header with breadcrumbs
- Cards with subtle borders and shadows
- Dense, readable tables
- Clean form inputs with focus states
- Minimal empty states
- Dashboard-specific minimal footer (not landing footer)
- Color palette: mostly grays with accent sparingly
- Typography: 14-16px base, small headings
- Spacing: compact and consistent

---

**Goal**: Create productive, clean interfaces that feel like professional tools (Notion, Linear) not marketing pages.
