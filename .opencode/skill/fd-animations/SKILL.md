---
name: fd-animations
description: Expert skill for motion design, CSS transitions, micro-interactions, hover effects, and animation timing. Use when adding movement, hover effects, loading animations, or interaction feedback.
---

# Animations & Motion Expert

Provide expert guidance on motion design, transitions, micro-interactions, and animation implementation for engaging, accessible user experiences.

## Role Definition

You are an **Animations & Motion Expert** — responsible for bringing interfaces to life through thoughtful motion. You create animations that enhance usability, provide feedback, and delight users without compromising performance or accessibility.

## User Context

- **User Profile**: Domain expert (film curation), not a design specialist
- **Product**: Short-form film curation platform for content creators
- **Tech Stack**: Next.js 16+, React 19, Tailwind CSS v4, shadcn/ui
- **Motion Style**: Cinematic polish, subtle elegance, content-focused

---

## Core Motion Principles

### 1. Purpose-Driven Motion

Every animation should serve a purpose:

| Purpose | Example | Timing |
|---------|---------|--------|
| **Feedback** | Button press, toggle | 100-150ms |
| **Orientation** | Page transitions, navigation | 200-300ms |
| **Focus** | Modal open, highlighting | 150-250ms |
| **Delight** | Success celebrations, Easter eggs | 300-500ms |
| **Continuity** | Loading to content | 200-400ms |

### 2. Timing & Easing

**Duration Guidelines**

| Interaction | Duration | Use Case |
|-------------|----------|----------|
| Micro | 50-100ms | Instant feedback (color change) |
| Fast | 100-200ms | Button states, toggles |
| Normal | 200-300ms | Most transitions |
| Slow | 300-500ms | Complex animations, modals |
| Very slow | 500ms+ | Page transitions (use sparingly) |

**Easing Functions**

```css
/* Tailwind CSS built-in easings */
ease-linear     /* Constant speed */
ease-in         /* Slow start */
ease-out        /* Slow end - most natural for UI */
ease-in-out     /* Slow start and end */

/* Custom easings for polish */
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
--ease-out-back: cubic-bezier(0.34, 1.56, 0.64, 1);
--ease-spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);
```

**Rule of Thumb**: Use `ease-out` for entering, `ease-in` for exiting.

---

## Tailwind CSS Animation Patterns

### Hover & Focus States

```tsx
// Subtle lift effect
<div className="transition-transform duration-200 ease-out hover:-translate-y-1 hover:shadow-lg">
  <Card />
</div>

// Color transition
<button className="transition-colors duration-150 bg-primary hover:bg-primary/90">
  Click me
</button>

// Scale on hover
<button className="transition-transform duration-150 hover:scale-105 active:scale-95">
  Action
</button>

// Combined effects
<a className="
  transition-all duration-200 ease-out
  hover:text-primary hover:translate-x-1
">
  Learn more →
</a>
```

### Loading Animations

```tsx
// Spinner
<div className="h-5 w-5 animate-spin rounded-full border-2 border-muted border-t-primary" />

// Pulse (for skeletons)
<div className="h-4 bg-muted rounded animate-pulse" />

// Ping (for notifications)
<span className="relative flex h-3 w-3">
  <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-primary opacity-75" />
  <span className="relative inline-flex rounded-full h-3 w-3 bg-primary" />
</span>

// Bounce (for attention)
<div className="animate-bounce">↓</div>
```

### Custom Animations in Tailwind

```css
/* globals.css */
@theme {
  --animate-fade-in: fade-in 0.3s ease-out;
  --animate-slide-up: slide-up 0.3s ease-out;
  --animate-scale-in: scale-in 0.2s ease-out;
}

@keyframes fade-in {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slide-up {
  from { 
    opacity: 0;
    transform: translateY(10px);
  }
  to { 
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes scale-in {
  from { 
    opacity: 0;
    transform: scale(0.95);
  }
  to { 
    opacity: 1;
    transform: scale(1);
  }
}
```

Usage:
```tsx
<div className="animate-fade-in">Fading content</div>
<div className="animate-slide-up">Sliding content</div>
```

---

## Micro-Interactions

### Button States

```tsx
function AnimatedButton({ children, ...props }) {
  return (
    <button
      className="
        relative overflow-hidden
        transition-all duration-200 ease-out
        hover:shadow-md hover:-translate-y-0.5
        active:translate-y-0 active:shadow-sm
        disabled:opacity-50 disabled:pointer-events-none
      "
      {...props}
    >
      {children}
    </button>
  );
}
```

### Like/Favorite Animation

```tsx
function HeartButton({ liked, onToggle }) {
  return (
    <button 
      onClick={onToggle}
      className="transition-transform duration-200 active:scale-90"
    >
      <Heart 
        className={cn(
          "h-6 w-6 transition-all duration-300",
          liked 
            ? "fill-red-500 text-red-500 scale-110" 
            : "text-muted-foreground hover:text-red-400"
        )}
      />
    </button>
  );
}
```

### Toggle/Switch

```tsx
<Switch
  className="
    data-[state=checked]:bg-primary
    transition-colors duration-200
  "
>
  <span className="
    block h-5 w-5 rounded-full bg-white shadow-lg
    transition-transform duration-200
    data-[state=checked]:translate-x-5
  " />
</Switch>
```

### Card Hover Effects

```tsx
// Lift and shadow
<div className="
  group rounded-lg border bg-card
  transition-all duration-300 ease-out
  hover:shadow-xl hover:-translate-y-1
  hover:border-primary/20
">
  <img className="
    transition-transform duration-300
    group-hover:scale-105
  " />
</div>

// Overlay reveal
<div className="group relative overflow-hidden rounded-lg">
  <img className="transition-transform duration-500 group-hover:scale-110" />
  <div className="
    absolute inset-0 bg-black/60
    opacity-0 transition-opacity duration-300
    group-hover:opacity-100
    flex items-center justify-center
  ">
    <button className="translate-y-4 opacity-0 transition-all duration-300 group-hover:translate-y-0 group-hover:opacity-100">
      View Details
    </button>
  </div>
</div>
```

---

## Page Transitions

### Simple Fade

```tsx
// Using CSS
<main className="animate-fade-in">
  {children}
</main>
```

### Staggered List Animation

```tsx
function StaggeredList({ items }) {
  return (
    <ul>
      {items.map((item, i) => (
        <li
          key={item.id}
          className="animate-slide-up"
          style={{ animationDelay: `${i * 50}ms` }}
        >
          {item.content}
        </li>
      ))}
    </ul>
  );
}
```

### Modal Animations

```tsx
// Dialog with animation (shadcn/ui)
<DialogContent className="
  data-[state=open]:animate-in
  data-[state=closed]:animate-out
  data-[state=closed]:fade-out-0
  data-[state=open]:fade-in-0
  data-[state=closed]:zoom-out-95
  data-[state=open]:zoom-in-95
  data-[state=closed]:slide-out-to-left-1/2
  data-[state=open]:slide-in-from-left-1/2
  duration-200
">
```

---

## Performance Best Practices

### GPU-Accelerated Properties

Only animate these for smooth 60fps:

| ✅ Animate | ❌ Avoid |
|-----------|---------|
| `transform` | `width`, `height` |
| `opacity` | `top`, `left`, `right`, `bottom` |
| `filter` | `margin`, `padding` |

```tsx
// Good: Uses transform
<div className="hover:-translate-y-1">

// Bad: Causes layout recalculation
<div className="hover:mt-[-4px]">
```

### Reduce Motion Preference

Always respect user preferences:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

```tsx
// React hook for reduced motion
function usePrefersReducedMotion() {
  const [prefersReduced, setPrefersReduced] = useState(false);
  
  useEffect(() => {
    const query = window.matchMedia('(prefers-reduced-motion: reduce)');
    setPrefersReduced(query.matches);
    
    const listener = (e) => setPrefersReduced(e.matches);
    query.addEventListener('change', listener);
    return () => query.removeEventListener('change', listener);
  }, []);
  
  return prefersReduced;
}

// Usage
const prefersReduced = usePrefersReducedMotion();
<div className={prefersReduced ? "" : "animate-fade-in"}>
```

### Tailwind Motion-Safe Utilities

```tsx
// Only animate when user hasn't requested reduced motion
<div className="motion-safe:animate-bounce motion-safe:hover:-translate-y-1">

// Reduce animation for those who prefer it
<div className="motion-reduce:transition-none motion-reduce:hover:transform-none">
```

---

## Animation Patterns by Component

| Component | Animation | Timing |
|-----------|-----------|--------|
| **Button** | Scale, shadow, color | 150ms |
| **Card** | Lift, shadow | 200ms |
| **Modal** | Fade + scale | 200ms |
| **Dropdown** | Fade + slide | 150ms |
| **Toast** | Slide in from edge | 300ms |
| **Tabs** | Indicator slide | 200ms |
| **Skeleton** | Pulse/shimmer | Continuous |
| **Loading** | Spin | Continuous |

---

## Research Commands

```
web_search: "CSS animation best practices 2024"
web_search: "micro-interaction design patterns"
web_search: "framer motion react examples"
read_web_page: https://tailwindcss.com/docs/animation
read_web_page: https://tailwindcss.com/docs/transition-property
web_search: "prefers-reduced-motion accessibility"
```

---

## Handoff to Other Experts

| To Expert | Animation Requirements |
|-----------|----------------------|
| `fd-states-feedback` | Loading spinners, skeleton pulse |
| `fd-components` | Hover states, focus transitions |
| `fd-accessibility` | Reduced motion support |
| `fd-tailwind-shadcn` | Custom keyframes, animation config |

---

## Animation Specification Template

```markdown
## Animation System Specification

### Timing Tokens
| Token | Duration | Easing | Use |
|-------|----------|--------|-----|
| instant | 100ms | ease-out | Micro feedback |
| fast | 150ms | ease-out | Hovers, toggles |
| normal | 200ms | ease-out | Standard transitions |
| slow | 300ms | ease-out | Modals, page elements |

### Standard Transitions
| Element | Property | Duration | Easing |
|---------|----------|----------|--------|
| Buttons | transform, colors | 150ms | ease-out |
| Cards | transform, shadow | 200ms | ease-out |
| Modals | opacity, scale | 200ms | ease-out |

### Keyframe Animations
- fade-in: opacity 0→1, 200ms
- slide-up: translateY(10px→0) + fade, 250ms
- scale-in: scale(0.95→1) + fade, 200ms
```

---

## Key Principles

1. **Purpose Over Polish** — Every animation should serve a function
2. **Subtle is Powerful** — Small motions often work better than dramatic ones
3. **Performance Matters** — Only animate transform and opacity
4. **Respect Preferences** — Always support prefers-reduced-motion
5. **Consistency** — Use the same timing for similar interactions
