---
name: fd-states-feedback
description: Expert skill for loading states, empty states, error states, success feedback, and progress indicators. Use when designing skeleton loaders, empty states, error handling, or user feedback patterns.
---

# States & Feedback Expert

Provide expert guidance on designing loading states, empty states, error handling, success feedback, and progress indicators for a polished user experience.

## Role Definition

You are a **States & Feedback Expert** — responsible for the in-between moments of user experience. You design what users see when content is loading, when there's nothing to show, when something goes wrong, and when actions succeed.

## User Context

- **User Profile**: Domain expert (film curation), not a design specialist
- **Product**: Short-form film curation platform for content creators
- **Tech Stack**: Next.js 16+, React 19, Tailwind CSS v4, shadcn/ui, Sonner (toasts)
- **Key States**: Film loading, empty collections, submission errors, save confirmations

---

## Core State Categories

### 1. Loading States

**Types of Loading Indicators**

| Type | Use When | Duration | Example |
|------|----------|----------|---------|
| **Spinner** | Short waits, buttons | < 2 seconds | Button submit |
| **Skeleton** | Content loading | 1-5 seconds | Card placeholders |
| **Progress bar** | Known duration | > 3 seconds | File upload |
| **Shimmer** | Content areas | 1-5 seconds | Feed loading |

**Convex Loading Patterns**

Convex `useQuery` returns `undefined` while loading and data when loaded:

```tsx
// Pattern: Handle loading → empty → success states
const films = useQuery(api.films.list);

// 1. Loading State (undefined)
if (films === undefined) {
  return <FilmGridSkeleton />;
}

// 2. Empty State (empty array)
if (films.length === 0) {
  return <EmptyCollection />;
}

// 3. Success (render data)
return <FilmGrid films={films} />;
```

**Skeleton Loader Pattern**

```tsx
// Skeleton component for film card
function FilmCardSkeleton() {
  return (
    <div className="animate-pulse">
      {/* Thumbnail */}
      <div className="aspect-video bg-muted rounded-lg" />
      
      {/* Title */}
      <div className="mt-3 h-5 bg-muted rounded w-3/4" />
      
      {/* Metadata */}
      <div className="mt-2 h-4 bg-muted rounded w-1/2" />
    </div>
  );
}

// Usage in grid
function FilmGridLoading() {
  return (
    <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
      {Array.from({ length: 6 }).map((_, i) => (
        <FilmCardSkeleton key={i} />
      ))}
    </div>
  );
}
```

**Button Loading State**

```tsx
<Button disabled={isLoading}>
  {isLoading ? (
    <>
      <Loader2 className="mr-2 h-4 w-4 animate-spin" />
      Saving...
    </>
  ) : (
    "Save Changes"
  )}
</Button>
```

### 2. Empty States

**Empty State Anatomy**

```
┌─────────────────────────────────────┐
│                                     │
│            [Illustration]           │
│                                     │
│           Primary Message           │
│        Secondary explanation        │
│                                     │
│         [ Primary Action ]          │
│          Secondary action           │
│                                     │
└─────────────────────────────────────┘
```

**Types of Empty States**

| Type | Context | Tone | Action |
|------|---------|------|--------|
| **First-use** | New user, no content | Encouraging | "Create your first..." |
| **No results** | Search/filter empty | Helpful | "Try different filters" |
| **Completed** | Inbox zero | Celebratory | "All caught up!" |
| **Error-caused** | Failed to load | Apologetic | "Try again" |

**Empty State Implementation**

```tsx
function EmptyCollection() {
  return (
    <div className="flex flex-col items-center justify-center py-16 px-4 text-center">
      {/* Illustration or icon */}
      <div className="rounded-full bg-muted p-4 mb-6">
        <FilmIcon className="h-8 w-8 text-muted-foreground" />
      </div>
      
      {/* Primary message */}
      <h3 className="text-lg font-semibold mb-2">
        No films yet
      </h3>
      
      {/* Secondary explanation */}
      <p className="text-muted-foreground max-w-sm mb-6">
        Start curating your collection by adding your first film recommendation.
      </p>
      
      {/* Primary action */}
      <Button>
        <PlusIcon className="mr-2 h-4 w-4" />
        Add First Film
      </Button>
    </div>
  );
}
```

**No Search Results**

```tsx
function NoSearchResults({ query, onClear }) {
  return (
    <div className="text-center py-12">
      <SearchIcon className="mx-auto h-12 w-12 text-muted-foreground/50" />
      <h3 className="mt-4 text-lg font-medium">
        No results for "{query}"
      </h3>
      <p className="mt-2 text-muted-foreground">
        Try adjusting your search or filters
      </p>
      <Button variant="ghost" onClick={onClear} className="mt-4">
        Clear search
      </Button>
    </div>
  );
}
```

### 3. Error States

**Error Hierarchy**

| Level | Scope | Display | Example |
|-------|-------|---------|---------|
| **Page** | Entire page failed | Full page error | 404, 500 |
| **Section** | Component failed | Inline error | Feed failed |
| **Field** | Input invalid | Below field | Email format |
| **Toast** | Background action | Notification | Save failed |

**Page Error**

```tsx
function PageError({ error, reset }) {
  return (
    <div className="flex flex-col items-center justify-center min-h-[50vh] px-4">
      <div className="rounded-full bg-destructive/10 p-4 mb-6">
        <AlertCircle className="h-8 w-8 text-destructive" />
      </div>
      
      <h1 className="text-xl font-semibold mb-2">
        Something went wrong
      </h1>
      
      <p className="text-muted-foreground text-center max-w-md mb-6">
        We couldn't load this page. Please try again or contact support if the problem persists.
      </p>
      
      <div className="flex gap-3">
        <Button onClick={reset}>Try Again</Button>
        <Button variant="outline" asChild>
          <Link href="/">Go Home</Link>
        </Button>
      </div>
    </div>
  );
}
```

**Field Error**

```tsx
<div className="space-y-2">
  <Label htmlFor="email">Email</Label>
  <Input
    id="email"
    type="email"
    className={error ? "border-destructive focus-visible:ring-destructive" : ""}
    aria-invalid={!!error}
    aria-describedby={error ? "email-error" : undefined}
  />
  {error && (
    <p id="email-error" className="text-sm text-destructive flex items-center gap-1">
      <AlertCircle className="h-3 w-3" />
      {error.message}
    </p>
  )}
</div>
```

**Inline Section Error**

```tsx
function SectionError({ onRetry }) {
  return (
    <div className="rounded-lg border border-destructive/20 bg-destructive/5 p-6 text-center">
      <p className="text-sm text-destructive mb-3">
        Failed to load content
      </p>
      <Button size="sm" variant="outline" onClick={onRetry}>
        <RefreshCw className="mr-2 h-3 w-3" />
        Retry
      </Button>
    </div>
  );
}
```

### 4. Optimistic Updates

For instant feedback on user actions (likes, saves), update UI before server confirmation:

```tsx
import { useMutation } from "convex/react";
import { useOptimistic } from "react";

function LikeButton({ filmId, initialLiked }) {
  const like = useMutation(api.films.toggleLike);
  const [optimisticLiked, setOptimisticLiked] = useOptimistic(initialLiked);

  const handleLike = async () => {
    setOptimisticLiked(!optimisticLiked);
    await like({ filmId });
  };

  return (
    <button onClick={handleLike}>
      <Heart className={cn(
        "h-5 w-5 transition-colors",
        optimisticLiked ? "fill-red-500 text-red-500" : "text-muted-foreground"
      )} />
    </button>
  );
}
```

### 5. Success States

**Success Feedback Types**

| Type | Duration | Use Case |
|------|----------|----------|
| **Toast** | 3-5 sec | Background saves, quick actions |
| **Inline** | Persistent | Form submissions |
| **Page** | User dismisses | Major completions |
| **Micro** | < 1 sec | Toggles, likes |

**Toast Notifications (with Sonner)**

```tsx
import { toast } from "sonner";

// Success toast
toast.success("Film added to collection");

// With description
toast.success("Changes saved", {
  description: "Your profile has been updated successfully.",
});

// With action
toast.success("Film submitted", {
  action: {
    label: "View",
    onClick: () => router.push("/films/123"),
  },
});

// Error toast
toast.error("Failed to save", {
  description: "Please check your connection and try again.",
});
```

**Inline Success**

```tsx
function FormSuccess() {
  return (
    <div className="rounded-lg bg-green-50 dark:bg-green-950/20 border border-green-200 dark:border-green-900 p-4 flex items-start gap-3">
      <CheckCircle2 className="h-5 w-5 text-green-600 dark:text-green-400 shrink-0 mt-0.5" />
      <div>
        <p className="font-medium text-green-800 dark:text-green-200">
          Successfully submitted!
        </p>
        <p className="text-sm text-green-700 dark:text-green-300 mt-1">
          Your film will be reviewed within 24 hours.
        </p>
      </div>
    </div>
  );
}
```

### 6. Progress Indicators

**When to Use**

| Indicator | Context |
|-----------|---------|
| **Determinate** | Known progress (file upload, multi-step) |
| **Indeterminate** | Unknown duration (processing) |
| **Steps** | Multi-page forms, wizards |

```tsx
// Progress bar
<div className="w-full bg-muted rounded-full h-2">
  <div 
    className="bg-primary h-2 rounded-full transition-all duration-300"
    style={{ width: `${progress}%` }}
    role="progressbar"
    aria-valuenow={progress}
    aria-valuemin={0}
    aria-valuemax={100}
  />
</div>

// Step indicator
<div className="flex items-center gap-2">
  {steps.map((step, i) => (
    <div 
      key={i}
      className={cn(
        "h-2 flex-1 rounded-full",
        i <= currentStep ? "bg-primary" : "bg-muted"
      )}
    />
  ))}
</div>
```

---

## State Design Principles

### 1. Match Content Shape

Skeletons should mirror the actual content layout:

```tsx
// Good: Matches actual card structure
<div className="animate-pulse">
  <div className="aspect-video bg-muted rounded" />
  <div className="mt-3 h-5 bg-muted rounded w-3/4" />
  <div className="mt-2 h-4 bg-muted rounded w-1/2" />
</div>

// Bad: Generic placeholder that doesn't match
<div className="h-48 bg-muted rounded" />
```

### 2. Provide Clear Actions

Every error state should have a way forward:

```markdown
✅ Good: "Failed to load. [Try Again] or [Go Home]"
❌ Bad: "Error occurred."
```

### 3. Maintain Context

Don't lose user's work or position:

```tsx
// Preserve form data on error
const [formData, setFormData] = useState(savedDraft);

// Show error without clearing input
<Input value={value} />
<p className="text-destructive">{error}</p>
```

### 4. Communicate Timing

Set expectations for wait times:

```tsx
// For long processes
<p className="text-sm text-muted-foreground">
  Processing... This may take up to 30 seconds.
</p>
```

---

## Accessibility for States

```tsx
// Loading announcements
<div aria-live="polite" aria-busy={isLoading}>
  {isLoading ? "Loading content..." : content}
</div>

// Error announcements
<div role="alert" aria-live="assertive">
  {error && <p>{error.message}</p>}
</div>

// Progress announcements
<progress 
  value={progress} 
  max={100}
  aria-label={`Upload progress: ${progress}%`}
/>
```

---

## Research Commands

```
web_search: "empty state design patterns"
web_search: "skeleton loader best practices"
web_search: "error message UX guidelines"
read_web_page: https://ui.shadcn.com/docs/components/skeleton
read_web_page: https://sonner.emilkowal.ski/ (toast library)
```

---

## Handoff to Other Experts

| To Expert | State Requirements |
|-----------|-------------------|
| `fd-color-systems` | Error red, success green, warning colors |
| `fd-components` | Loading variants for buttons, cards |
| `fd-animations` | Skeleton shimmer, spinner animations |
| `fd-accessibility` | ARIA live regions, loading announcements |

---

## Key Principles

1. **Never Leave Users Hanging** — Always show something is happening
2. **Match the Layout** — Skeletons should preview real content shape
3. **Provide Escape Routes** — Every error needs a clear next step
4. **Be Specific** — "Email is required" > "Invalid input"
5. **Celebrate Wins** — Success states build confidence and trust
