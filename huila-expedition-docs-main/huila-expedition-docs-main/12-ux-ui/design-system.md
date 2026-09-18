# Design System

> The design system is the shared visual language between design and development.
> It prevents inconsistencies, accelerates design, and reduces rework.
> *Rule:* Before creating a new component, check here if it already exists.

---

## Design tokens

Tokens are the design system's variables. Changing a token changes the entire system.

### Colors

css
/* Base palette */
--color-primary-50:  #E8F5E9;   /* Lightest SENA Green tint */
--color-primary-100: #C8E6C9;
--color-primary-500: #39A900;   /* Default — SENA Corporate Green */
--color-primary-900: #1B5E20;   /* Darkest SENA Green shade */

--color-secondary-500: #00324D; /* Complementary dark blue from institutional emblem */
--color-neutral-50:  #F8F9FA;   /* Light application background */
--color-neutral-900: #212529;   /* Dark primary text color */

/* Semantic colors */
--color-success:  #28A745;      /* Green — success, available / booking request successfully sent */
--color-warning:  #FFC107;      /* Yellow — caution, few spots left / booking approval pending status */
--color-error:    #DC3545;      /* Red — no slots left, error, booking cancelled */
--color-info:     #17A2B8;      /* Blue — general system information or support help */

/* Text */
--color-text-primary:   #212529; /* High contrast primary text */
--color-text-secondary: #6C757D; /* Secondary text, subtitles, or field labels */
--color-text-disabled:  #ADB5BD; /* Disabled state of interactive elements */

/* Backgrounds */
--color-bg-page:    #F8F9FA;     /* Main web platform background */
--color-bg-card:    #FFFFFF;     /* Background for tour plan cards */
--color-bg-overlay: rgba(33, 37, 41, 0.5); /* Semitransparent backdrop for modals (Login/Booking) */


### Typography

css
/* Families (Based on responsive specifications with Bootstrap / Tailwind CSS from SRS) */
--font-family-sans:  'system-ui', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
--font-family-mono:  'SFMono-Regular', Menlo, Monaco, Consolas, monospace;

/* Sizes (modular scale 1.25) */
--font-size-xs:   0.75rem;   /* 12px */
--font-size-sm:   0.875rem;  /* 14px */
--font-size-base: 1rem;      /* 16px */
--font-size-lg:   1.25rem;   /* 20px */
--font-size-xl:   1.563rem;  /* 25px */
--font-size-2xl:  1.953rem;  /* 31px */
--font-size-3xl:  2.441rem;  /* 39px */

/* Weights */
--font-weight-regular: 400;
--font-weight-medium:  500;
--font-weight-bold:    700;

/* Line height */
--line-height-tight:  1.2;
--line-height-normal: 1.5;
--line-height-loose:  1.8;


### Spacing

css
/* 4px system */
--space-1:  0.25rem;   /* 4px */
--space-2:  0.5rem;    /* 8px */
--space-3:  0.75rem;   /* 12px */
--space-4:  1rem;      /* 16px */
--space-6:  1.5rem;    /* 24px */
--space-8:  2rem;      /* 32px */
--space-12: 3rem;      /* 48px */
--space-16: 4rem;      /* 64px */


### Borders and shadows

css
/* Border radius */
--radius-sm: 4px;
--radius-md: 8px;
--radius-lg: 16px;
--radius-full: 9999px;  /* Pill */

/* Shadows */
--shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
--shadow-md: 0 4px 6px rgba(0,0,0,0.1);
--shadow-lg: 0 10px 15px rgba(0,0,0,0.15);


---

## Components

### Buttons

| Variant | Use | Disabled state |
|---------|-----|----------------|
| Primary | Main action on the page | opacity: 0.5; cursor: not-allowed |
| Secondary | Secondary actions | same |
| Danger | Destructive actions (delete) | same |
| Ghost | Tertiary actions, links | same |

*Usage rules:*
- Only one Primary action per view
- Danger only with modal confirmation ("Are you sure?")
- Buttons have a loading state for async operations

### Forms

| Component | When to use |
|-----------|-------------|
| Input text | Single-line free text |
| Textarea | Multi-line free text |
| Select | Fixed list of options (< 15 items) |
| Combobox | List with search (> 15 items or dynamic loading) |
| Checkbox | Independent binary option |
| Radio | Select one option from a few (2-5) |
| Toggle | Enable/disable a feature |
| DatePicker | Date selection |

*Error messages in forms:*
- The message appears below the field, in red
- The field border turns red
- The message says how to fix the error, not just that there is an error

### Feedback

| Component | When | Duration |
|-----------|------|---------|
| Toast/Snackbar | Action confirmations | 4 seconds |
| Inline alert | Form errors | Until corrected |
| Modal | Destructive confirmations, irreversible actions | Until the user decides |
| Loading spinner | Operations > 200ms | Until finished |
| Skeleton | Loading list content / cards | Until loaded |

### Data table

| Aspect | Behavior |
|--------|---------|
| Pagination | Maximum 20 rows per page (user-configurable) |
| Sorting | Click on column, toggle asc/desc |
| Filters | Side panel or filter row above the table |
| Selection | Checkbox in the first column |
| Actions | Final column with actions menu (edit, delete, etc.) |
| Empty state | Illustration + message + primary action CTA |

---

## UX patterns

### Principles

1. *Confirm before destroying:* Any action that permanently deletes or modifies data requires a confirmation modal.

2. *Immediate feedback:* Every action must have a visual response in < 100ms (even if it is just the loading state).

3. *Prevent rather than correct:* Validate in real time in the form, not only on submit.

4. *Empty state as a feature:* The screen without data is the new user's first impression — guide them to the first action.

### Error handling

| Scenario | What to show |
|----------|-------------|
| Network error | Toast "No connection. Retrying..." with automatic retry |
| 401 error | Redirect to login with message "Your session expired" |
| 403 error | Screen "You do not have permission to view this" with link to support |
| 404 error | 404 screen with back navigation |
| 500 error | Error toast + "Retry" button |
| Timeout | Toast "This is taking longer than normal" with cancel option |

---

## Accessibility guide (minimums)

| Aspect | Minimum required |
|--------|-----------------|
| Text contrast | WCAG AA (4.5:1 for normal text, 3:1 for large text) |
| Keyboard navigation | All interactive elements accessible with Tab |
| Form labels | All fields with associated label (for / aria-label) |
| Images | Descriptive alt text on all non-decorative images |
| Visible focus | Visible focus indicator on all interactive elements |

---

## Correlations

- Navigation map → 12-ux-ui/navigation-map.md
- Wireframes → 12-ux-ui/wireframes.md
- UX non-functional requirements → 04-requirements/non-functional.md
