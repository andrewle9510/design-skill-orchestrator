# Design Skill Orchestrator

It's level-up version of [frontend-design](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/frontend-design/skills/frontend-design/SKILL.md) of Anthropic, not fast-food template, not ai-looking-style template. It's knowledge for your agents.

## How to Use

**Call the `frontend-design` skill and describe what you need:**

```
frontend-design create a pricing page
frontend-design make a login button
frontend-design add dark mode
frontend-design improve my landing page
```

That's it. The system handles the rest.

## How It Works

1. **frontend-design is an orchestrator** - It understands your request and coordinates the right experts
2. **Experts are called automatically** - It figures out which specialist skills are needed:
   - `fd-color-systems` - Colors, themes, dark mode
   - `fd-typography` - Fonts, text, readability
   - `fd-spacing-layout` - Layouts, grids, responsive design
   - `fd-components` - UI elements like buttons, cards, forms
   - `fd-animations` - Hover effects, transitions
   - `fd-patterns` - Page layouts, navigation
   - `fd-accessibility` - Screen readers, keyboard navigation
   - `fd-states-feedback` - Loading, error, empty states
   - `fd-tailwind-shadcn` - Tailwind CSS styling
   - `fd-design-philosophy` - Visual style and brand feel
3. **You get options** - Clear choices explained simply, no design jargon

## For Non-Designers

You don't need to know:
- Which colors work together
- Proper spacing or font sizes
- Accessibility requirements
- CSS tricks or Tailwind classes

Just describe what you're building and what you want it to do. The system will:
- Suggest design approaches
- Show you options to choose from
- Explain things in plain language
- Handle the technical details

## Quick Example

**You say:** `frontend-design help me create a pricing card for my SaaS`

**System does:**
- Calls layout expert → suggests grid positioning
- Calls color expert → proposes color scheme
- Calls typography expert → recommends text sizes
- Calls component expert → designs the card structure
- Calls animation expert → adds hover effects

**You see:** "Here are 3 pricing card styles. Option A is clean and minimal, Option B emphasizes savings with accent colors, Option C uses a modern gradient. Which fits your brand?"

## What You Can Ask For

- Pages, sections, layouts
- Buttons, cards, forms, modals
- Navigation, menus, sidebars
- Themes (dark/light mode)
- Loading states, error messages
- Any visual improvement or change

## Requirements

- OpenCode AI assistant (or compatible platform)
- Next.js 16+ project
- Tailwind CSS v4
- shadcn/ui components

## The Skills

| Skill | Purpose |
|-------|---------|
| `frontend-design` | Main entry point - call this one |
| `fd-*` | Expert skills (called automatically) |
