# KISS, DRY and scalability in practice

Contents:
1. Deciding whether to extract
2. Worked examples
3. Where shared code lives
4. When DRY goes wrong
5. Refactor recipe for existing code

## 1. Deciding whether to extract

Ask these in order:

1. **Is it the same piece of knowledge?** (the same business rule, limit, format) If yes, centralize now, even on the second use.
2. **Is it merely similar-looking code?** If the two copies change for different reasons, leave them separate. Revisit on the third occurrence.
3. **Would a reader find the abstraction easier than the inline code?** If the helper name is no clearer than the code it replaces, do not extract. `const double = (n) => n * 2` used once is noise.
4. **Does it vary by environment or by customer?** Then it is configuration, not a constant.

Rule of thumb: centralize *values and rules* eagerly; abstract *behavior* cautiously.

## 2. Worked examples

### Limits and timeouts

```ts
// constants/timing.ts
export const DEBOUNCE_MS = 300;
export const REQUEST_TIMEOUT_MS = 10_000;
export const MAX_RETRIES = 3;
```

Put the unit in the name (`_MS`, `_SECONDS`) so no one has to guess.

### Routes and API paths

```ts
// constants/routes.ts
export const ROUTES = {
  home: '/',
  user: (id: string) => `/users/${id}`,
  settings: '/settings',
} as const;
```

Changing a path now touches one file. Links, redirects and tests all import `ROUTES`.

### Status and role strings

```ts
// constants/status.ts
export const ORDER_STATUS = {
  pending: 'pending',
  paid: 'paid',
  shipped: 'shipped',
} as const;
export type OrderStatus = (typeof ORDER_STATUS)[keyof typeof ORDER_STATUS];
```

Typos in a repeated string become compile errors instead of silent bugs.

### Pagination (end to end)

```ts
// helpers/pagination.ts
export const DEFAULT_PAGE_SIZE = 25;
export const MAX_PAGE_SIZE = 100;

export function clampPageSize(requested?: number): number {
  if (!requested || requested < 1) return DEFAULT_PAGE_SIZE;
  return Math.min(requested, MAX_PAGE_SIZE);
}

export function getPageCount(total: number, pageSize = DEFAULT_PAGE_SIZE): number {
  return Math.max(1, Math.ceil(total / pageSize));
}
```

The UI component, the API handler and the tests all import the same values. Raising the default from 25 to 50 is a one-line change.

### CSS tokens

```css
:root {
  --space-2: 0.5rem;
  --space-4: 1rem;
  --color-primary: #1a56db;
  --radius-md: 0.5rem;
}
.card { padding: var(--space-4); border-radius: var(--radius-md); }
```

The same principle: name the value once, reuse it.

## 3. Where shared code lives

Follow what the repo already does. If it has nothing, a simple default is:

```
src/
  helpers/        # pure functions, grouped by domain
    index.ts      # re-exports
    pagination.ts
    format.ts
  constants/      # values and enums-as-objects
    index.ts
    routes.ts
    timing.ts
  config/         # env-dependent values, read once, typed
```

Notes:

- Keep helpers **pure** and small. Side effects (network, storage) go in a service layer, not in `helpers`.
- Barrel files (`index.ts` re-exporting a folder) keep imports short. In very large codebases they can hurt bundling or create circular imports; if that appears, import from the specific file.
- Use the project's path aliases (`@/helpers`) when they exist.
- Do not create `utils.ts` as a dumping ground. Name modules after their domain.

## 4. When DRY goes wrong

Symptoms of over-abstraction, which KISS warns against:

- A helper with many boolean flags or options to cover callers that are not really the same case.
- A base class or generic component that every user bends with overrides.
- A shared function that gets a new parameter every time one caller needs something slightly different.
- Constants for things that never change and have one use.

Fix: split the abstraction back into specific pieces, and share only the genuinely common core (often a small helper or a constant, not a whole component).

Duplication is cheaper than the wrong abstraction. Wait for a clear shared shape before merging.

## 5. Refactor recipe for existing code

When touching code that already has scattered literals or copies:

1. Grep for the literal or the repeated snippet to find every use.
2. Decide whether they are the same knowledge (merge) or look-alikes (leave).
3. Create or extend the shared module; name the constant by meaning, with a unit if needed.
4. Replace all occurrences in one pass so no stale copies remain.
5. Run the tests and the type checker; behavior must not change.
6. Keep the diff focused: a refactor commit should not also add features.

For naming smells and the classic fixes (Extract Method, Replace Magic Number with Symbolic Constant, Extract Class, and others), see `references/design-patterns.md` and the refactoring section of `references/sources.md`.
