---
name: frontend-aesthetics
description: Prevents generic AI-generated designs by guiding typography, color, motion, and background choices. Use when creating frontend designs, landing pages, dashboards, or any UI/UX work. Helps avoid the "AI slop" aesthetic.
---

# Frontend Aesthetics Skill

Based on Anthropic's formula for improving frontend design through steerability.

## The Problem

LLMs converge toward generic, "on distribution" outputs. In frontend design, this creates what users call the "AI slop" aesthetic - Inter fonts, purple gradients on white backgrounds, and minimal animations.

## Instructions

Make creative, distinctive frontends that surprise and delight. Focus on these four dimensions:

### Typography

Choose fonts that are beautiful, unique, and interesting.

**Never use**: Inter, Roboto, Open Sans, Lato, Arial, default system fonts

**Good choices**:
- Code aesthetic: JetBrains Mono, Fira Code, Space Grotesk
- Editorial: Playfair Display, Crimson Pro, Newsreader
- Technical: IBM Plex family, Source Sans 3
- Distinctive: Bricolage Grotesque, Syne, Outfit, Plus Jakarta Sans
- Premium: Cabinet Grotesk, Satoshi, General Sans, Clash Display

**Pairing principle**: High contrast = interesting. Display + monospace, serif + geometric sans, variable font across weights.

**Use extremes**: 100/200 weight vs 800/900, not 400 vs 600. Size jumps of 3x+, not 1.5x.

Pick one distinctive font, use it decisively. Load from Google Fonts.

### Bold Header Design

Landing page heroes must COMMAND attention, not politely ask for it.

**Full Viewport Heroes**:
- `min-h-screen` or `h-screen` for impact
- Center content vertically and horizontally
- Don't settle for 60vh

**Headline Sizes** (go BOLD):
- Hero headlines: `text-6xl md:text-7xl lg:text-8xl xl:text-9xl`
- Not timid: Avoid `text-4xl` or `text-5xl` for main hero
- Mobile: Still bold at `text-5xl` minimum

**Gradient Text Effects**:
```tsx
<h1 className="text-8xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
  Your Headline
</h1>
```

**Animated Text Reveals**:
- Fade in + slide up on page load
- Letter-by-letter reveals for premium feel
- Gradient animation within text

**Floating Elements/Badges**:
- Trust badges ("Trusted by 10k+ users") floating near headline
- Animated icons orbiting the hero
- Geometric shapes as background accent
- Use `absolute` positioning with z-index layering

**Hero Anti-Patterns**:
- Generic centered text on plain background
- Small, timid headlines
- No visual hierarchy
- Static, lifeless presentation
- Uniform spacing (vary spacing dramatically)

### Color & Theme

Commit to a cohesive aesthetic. Use CSS variables for consistency.

**Dominant colors with sharp accents** outperform timid, evenly-distributed palettes.

#### Light Theme Guidance

When building light themes, use CONTRAST and BOLDNESS:

**Backgrounds**:
- #FAFAFA (off-white)
- #F9FAFB (cool gray-white)
- #FAF9F6 (cream)
- Subtle gradients: `from-white to-gray-50`

**Text**:
- slate-900, gray-900 for maximum contrast
- Never use gray-600 or lighter for body text

**Accent Colors** (must pop on light):
- Electric blue: #0066FF
- Hot pink: #FF006E
- Emerald: #10B981
- Amber: #F59E0B
- Avoid: muted pastels, washed-out purples, generic blues

**Light Theme Anti-Patterns**:
- Purple gradients on white (AI slop)
- Low-contrast gray text
- Uniform white background with no depth
- Pastel colors that blend into background

Draw inspiration from:
- IDE themes (Dracula, Nord, One Dark, Catppuccin, Tokyo Night)
- Cultural aesthetics (Japanese minimalism, Scandinavian design, Brutalism)
- Industry-specific palettes (Finance: navy/gold, Health: teal/white, Gaming: neon/dark)

### Motion

Use animations for effects and micro-interactions.

- Prioritize CSS-only solutions for HTML
- Use Motion library for React when available
- Focus on high-impact moments: one well-orchestrated page load with staggered reveals (animation-delay) creates more delight than scattered micro-interactions

#### Conversion-Focused Animation Patterns

**Staggered Reveal** (sequential entrance):
```css
@keyframes fade-in-up {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.stagger-1 { animation: fade-in-up 0.6s ease-out 0.1s both; }
.stagger-2 { animation: fade-in-up 0.6s ease-out 0.2s both; }
.stagger-3 { animation: fade-in-up 0.6s ease-out 0.3s both; }
```

**CTA Micro-Interactions**:
```css
.cta-button {
  transition: all 0.2s ease;
}
.cta-button:hover {
  transform: scale(1.05);
  box-shadow: 0 0 30px rgba(accent, 0.4);
}
```

**Background Effects**:
```css
@keyframes gradient-shift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.animated-bg {
  background: linear-gradient(270deg, color1, color2, color3);
  background-size: 600% 600%;
  animation: gradient-shift 15s ease infinite;
}
```

**Floating Elements**:
```css
@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-20px); }
}
.float { animation: float 3s ease-in-out infinite; }
```

**Scroll-Triggered**: Use Intersection Observer for reveals on scroll (not just page load)

### Backgrounds

Create atmosphere and depth rather than defaulting to solid colors.

- Layer CSS gradients (radial + linear combinations)
- Use geometric patterns or grids
- Add contextual effects that match the overall aesthetic
- Consider noise textures, grain, or subtle animations

## Tailwind CSS v4 Compatibility

**CRITICAL**: Tailwind v4 has breaking changes. Follow these rules:

### Spacing
- **Never use**: `space-x-*`, `space-y-*` (removed in v4)
- **Always use**: `gap-*` with flex/grid containers instead

```tsx
// WRONG (Tailwind v3 only)
<div className="flex space-x-4">

// CORRECT (Tailwind v4 compatible)
<div className="flex gap-4">
```

### Config Loading
- Tailwind v4 doesn't auto-load `tailwind.config.ts`
- Add `@config "../tailwind.config.ts"` to your CSS file if using a config

```css
/* app/globals.css */
@import "tailwindcss";
@config "../tailwind.config.ts";
```

### CSS Reset Conflicts
- Don't add custom `* { margin: 0 }` resets - they break `mx-auto`, `my-*` utilities
- Let Tailwind's preflight handle resets

### Package.json
- Add `"type": "module"` to avoid Node.js ESM warnings

## Landing Pages vs Dashboards

**Landing Pages = BOLD & DRAMATIC**:
- Full viewport heroes (100vh)
- Massive headlines (text-8xl, text-9xl)
- Gradient text, animated backgrounds
- Conversion-focused: every element drives to CTA
- Visual storytelling with scroll reveals
- Maximum contrast and impact

**Dashboards = CLEAN & FUNCTIONAL**:
- Information density over drama
- Consistent spacing and grid systems
- Readable font sizes (text-sm to text-2xl)
- Data visualization takes center stage
- Subtle animations (loading states, transitions)
- See `.claude/skills/dashboard-design/` for app UI guidance

**Don't mix contexts**: A dashboard shouldn't look like a landing page, and vice versa.

## What to Avoid

- Overused font families (Inter, Roboto, Arial, system fonts)
- Clichéd color schemes (purple gradients on white)
- Predictable layouts and component patterns
- Cookie-cutter design that lacks context-specific character
- Space Grotesk (even this is becoming overused)
- `space-x-*` and `space-y-*` utilities (use `gap-*` instead)
- Generic hero sections with no personality
- Uniform spacing (mix tight and loose)
- No animations or motion
- Muted, lifeless colors on light backgrounds
- Low contrast text (gray-600 on white)

## Key Principle

Interpret creatively and make unexpected choices that feel genuinely designed for the context. Vary between light and dark themes, different fonts, different aesthetics.

You still tend to converge on common choices across generations. Avoid this: it is critical that you think outside the box!

## Example: Theme Ideas

Instead of the default "tech purple":

### Dark Themes

1. **Obsidian Terminal**: Deep black (#0a0a0a), electric green (#00ff9f), JetBrains Mono
2. **Neon Brutalist**: Pure black, pure white, hot pink accent (#ff006e), IBM Plex Mono
3. **Retro Computing**: Amber on dark (#ffb000 on #1a1a1a), VT323 or IBM Plex Mono
4. **Cyberpunk**: Dark purple (#1a0f2e), neon cyan (#00f0ff), electric pink, Orbitron

### Light Themes

5. **Editorial Luxury**: Cream (#faf9f6), deep navy (#1a1a2e), Playfair Display + Source Sans
6. **Nordic Minimal**: Cool gray (#e5e5e5), slate blue (#475569), Outfit
7. **Warm Editorial**: Cream (#faf9f6), terracotta (#e07a5f), charcoal (#2d2d2d), Playfair Display
8. **Tech Minimal**: Pure white (#ffffff), black text (#0a0a0a), cyan accent (#00d9ff), Outfit or Space Grotesk
9. **Bold Modern**: White (#ffffff), electric blue (#0066FF), geometric shapes, Cabinet Grotesk
10. **Soft Gradient**: Light gradient bg (from-blue-50 to-purple-50), pastel accents, Plus Jakarta Sans

**Key**: Pick ONE theme and commit fully. Mix font weights, not themes.
