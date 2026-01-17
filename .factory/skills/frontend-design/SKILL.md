---
name: frontend-design
description: Frontend design orchestrator that analyzes design requests and delegates to specialized expert skills. Use when the user asks to build UI components, pages, layouts, or any visual interface work. Understands user's non-expert background and coordinates color, typography, spacing, animation, and component experts.
---

# Frontend Design Orchestrator

Orchestrates frontend design tasks by understanding user intent and delegating to specialized expert skills.

## 🎯 AUTO-DETECTION: When to Load This Skill

**ALWAYS load this skill when user message contains ANY of these signals:**

### Direct Design Keywords
- "change", "modify", "update", "improve", "fix" + UI element (header, footer, card, button, page, etc.)
- "make it", "looks", "style", "design", "beautiful", "ugly", "better", "worse"
- "another style", "different style", "try another"
- "review" + any UI/visual element

### UI Element Names
- header, navbar, nav, navigation, top bar, menu
- footer, sidebar, bottom bar
- card, button, input, form, modal, dialog, dropdown
- page, layout, section, hero, banner
- carousel, slider, grid, list

### Visual/Style Words
- color, font, spacing, padding, margin, size
- hover, animation, transition, effect
- dark, light, theme, mode
- responsive, mobile, desktop
- pretty, clean, modern, minimal, elegant

### Action + Visual Intent
- "help me change the..." → load skill
- "can you make..." → load skill  
- "I don't like the..." → load skill
- "try something else" (after visual change) → load skill

**If in doubt, load this skill.** It's better to consult experts than to skip them.

---

## ⚠️ CRITICAL: Always Follow Orchestration Workflow

**DO NOT skip expert skill delegation.** This orchestrator exists to coordinate experts, not to answer design questions directly.

Before responding to ANY design request:
1. **STOP** — Do not use oracle or answer directly
2. **ANALYZE** — Identify which expert domains are involved (Step 1 below)
3. **DELEGATE** — Load the appropriate expert skills using `skill` tool (Step 3 below)
4. **SYNTHESIZE** — Combine expert guidance into unified recommendation (Step 4 below)

**Anti-patterns to avoid:**
- ❌ Providing design recommendations without consulting expert skills
- ❌ Skipping expert skills because "it seems simple"
- ❌ Using only oracle without also delegating to expert skills

**Correct behavior:**
- ✅ Always load at least one expert skill for any design task
- ✅ Follow the Expert Sequence table for multi-domain requests
- ✅ Let experts provide domain-specific guidance
- ✅ Oracle can be used alongside expert skills for additional analysis, but not as a replacement

---

## User Context

**User Profile**: Domain expert, not a frontend/design specialist. Understands product vision but relies on expert guidance for design decisions.

**Tech Stack** (from AGENTS.md):
- Next.js 16+ with React 19
- Tailwind CSS v4
- shadcn/ui (base-lyra style)
- Lucide React icons
- React Compiler enabled

## Expert Skills Available

| Skill | Domain | When to Invoke |
|-------|--------|----------------|
| `fd-design-philosophy` | Core principles, aesthetics, brand identity | Defining visual direction, brand feel, design system foundations |
| `fd-color-systems` | Palettes, themes, dark mode | Color choices, theme switching, color semantics |
| `fd-typography` | Fonts, scale, hierarchy, readability | Text styling, font pairings, typographic scale |
| `fd-spacing-layout` | Grid, spacing, responsive breakpoints | Layout structure, whitespace, mobile-first, device adaptation |
| `fd-components` | UI components, variants, composition | Building specific components, shadcn/ui customization |
| `fd-animations` | Motion, transitions, micro-interactions | Adding movement, hover effects, feedback, polish |
| `fd-patterns` | Page compositions, navigation, forms | Full page designs, common UI patterns, user flows |
| `fd-accessibility` | WCAG, inclusive design, contrast | Screen readers, keyboard nav, focus states, aria labels |
| `fd-states-feedback` | Loading, empty, error, success states | Skeleton loaders, empty states, error handling, progress |
| `fd-tailwind-shadcn` | Tailwind CSS v4, shadcn/ui, tech implementation | CSS utilities, component customization, cn() usage, variants |

## Orchestration Workflow

### Step 1: Analyze the Request

When user makes a design request:

1. **Identify the scope** — Is this a component, page, system, or refinement?
2. **Detect domains involved** — Which expert areas are needed?
3. **Assess dependencies** — What must be decided first?
4. **Data strategy** — Is this static content or dynamic Convex data? (affects States & Components)

### Step 2: Determine Expert Sequence

Map the request to expert skills. Common patterns:

| Request Type | Expert Sequence |
|--------------|-----------------|
| "Build a new page" | Philosophy → Layout → Components → States → Animations |
| "Create a button component" | Components → Tailwind/shadcn → Animations |
| "Make it look better" | Philosophy → Color → Typography → Spacing |
| "Add dark mode" | Color Systems → Tailwind/shadcn |
| "Design a card grid" | Layout → Components → Patterns |
| "Add micro-interactions" | Animations |
| "Handle loading/error states" | States & Feedback → Components |
| "Make it accessible" | Accessibility → Color (contrast) → Components |
| "Build a form" | Patterns → Components → States → Accessibility |
| "Implement with Tailwind" | Tailwind/shadcn → (relevant design expert) |
| "Dynamic data page" | Patterns → States (Convex loading) → Components |

### Step 3: Invoke Expert Skills

For each identified domain, load the corresponding skill:

```
Use skill: fd-color-systems
```

Pass relevant context:
- What the user is trying to achieve
- Decisions already made by previous experts
- Constraints from tech stack

### Step 4: Synthesize & Present

After consulting experts:

1. **Summarize decisions** from each domain
2. **Present unified recommendation** to user
3. **Show visual preview** if applicable (code + description)
4. **Get user approval** before implementation

## Request Analysis Patterns

### Keywords → Expert Mapping

| Keywords | Primary Expert |
|----------|----------------|
| color, palette, theme, dark mode, contrast | `fd-color-systems` |
| font, typography, text, heading, readable | `fd-typography` |
| spacing, padding, margin, grid, layout, responsive, breakpoint, mobile | `fd-spacing-layout` |
| button, card, modal, input, form, component, variant | `fd-components` |
| animation, transition, hover, motion, interactive, micro-interaction | `fd-animations` |
| page, section, pattern, navigation, composition, flow | `fd-patterns` |
| style, aesthetic, vibe, feel, brand, identity, vision | `fd-design-philosophy` |
| accessible, a11y, wcag, screen reader, keyboard, focus, aria | `fd-accessibility` |
| loading, skeleton, empty, error, success, disabled, progress | `fd-states-feedback` |
| tailwind, shadcn, cn(), class, utility, css, implement, code | `fd-tailwind-shadcn` |

### Complexity Assessment

**Simple** (1 expert): Direct component or single-domain task
**Medium** (2-3 experts): Component with styling decisions
**Complex** (4+ experts): Full page or design system work

## Communication Style

When presenting to user:

- **Avoid jargon** — Explain design terms simply
- **Show options** — Let user choose between approaches
- **Visual examples** — Describe what it will look like
- **Incremental** — Break large designs into reviewable chunks

## Key Principles

1. **User is the decision maker** — Present, don't dictate
2. **Experts handle details** — Orchestrator coordinates, not implements
3. **Context flows forward** — Each expert builds on previous decisions
4. **Quality over speed** — Consult appropriate experts, don't shortcut
