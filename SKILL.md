---
name: dev-standards
description: Coding standards for developer agents building web software. Enforces KISS, DRY and scalability (no magic numbers or strings; shared constants and helpers instead of inline values such as page sizes, limits, timeouts, routes), semantic standards-compliant HTML/CSS/JS, and sensible use of design patterns, backed by curated references (WHATWG HTML spec, DevDocs, patterns.dev, refactoring.guru). Use this skill whenever writing, reviewing or refactoring frontend or backend code, creating components, paginators, forms, helpers, utils or constants, choosing a design pattern, or when the user mentions clean code, KISS, DRY, scalability, reusable code, refactoring, semantic HTML or best practices, even if they never name the skill.
---

# Dev standards: KISS, DRY and scalable code

Code gets read and changed far more often than it gets written. These standards exist so that a change to a rule (a page size, a timeout, a route) is made in one place, and so that the next developer or agent understands the code without reverse-engineering it.

## Workflow

Follow this order whenever you write or change code:

1. **Look before you create.** Search the repo for existing shared code (`helpers/`, `utils/`, `constants/`, `config/`, `shared/`, `lib/`) and for an existing component or function that already does the job. Reuse or extend it. Duplicating something that already exists is the most common DRY failure.
2. **Name every rule once.** If a value carries meaning (a limit, a delay, a status, a route, a regex, a storage key), it lives in one shared module and everything imports it. See "Magic values" below.
3. **Write the simplest thing that works (KISS).** Add an abstraction when there is a real second use or when the value is a policy that may change, not because it might be useful someday.
4. **Don't guess about the web platform or about patterns.** If you are unsure how an HTML element, a CSS feature or a JS API behaves, or which design pattern fits, consult the sources in `references/sources.md` and fetch the specific page. Do not fetch whole documents; the HTML spec alone is enormous.
5. **Run the checklist** at the end of this file before you hand the work back.

## Principles

**KISS.** Prefer the plainest solution that meets today's requirement. Fewer moving parts means fewer bugs and faster onboarding. A pattern, class hierarchy or config layer must earn its place by solving a problem you can name.

**DRY applies to knowledge, not to text.** Two snippets that look alike but change for different reasons should stay separate; merging them couples unrelated things. Two places that encode the same rule must become one. When in doubt, tolerate duplication once and extract on the third occurrence (rule of three), unless the duplicated thing is a business rule or a configurable value, in which case centralize immediately.

**Scalability by centralization.** Growth usually shows up as "the same thing, in more places, with slightly different values". Code that names its rules in one place and builds on small reusable helpers scales; code with the rule copied into each file does not.

## Magic values: constants and helpers, not inline literals

Do not repeat literal values that carry meaning. Create a shared module and import from it.

Bad: the same rule, scattered and inconsistently named.

```ts
// UsersList.tsx
const page_size = 25;
// ProductsList.tsx
const limit = 25;
```

Good: one source of truth, with a helper that holds the logic around it.

```ts
// helpers/pagination.ts
export const DEFAULT_PAGE_SIZE = 25;
export const MAX_PAGE_SIZE = 100;

export function clampPageSize(requested?: number): number {
  if (!requested || requested < 1) return DEFAULT_PAGE_SIZE;
  return Math.min(requested, MAX_PAGE_SIZE);
}

// helpers/index.ts
export * from './pagination';

// anywhere in the app
import { DEFAULT_PAGE_SIZE, clampPageSize } from '@/helpers';
```

Guidelines:

- **What to centralize:** page sizes and limits, debounce and timeout durations, retry counts, breakpoints, regexes, route paths, storage and cache keys, API paths, status and role strings, date formats, user-facing messages, feature flags.
- **What not to centralize:** values whose meaning is purely local (`0`, `1`, an array index, `% 2`). Extracting those makes code harder to read, which defeats KISS.
- **Different values for different roles:** name constants by meaning (`DEFAULT_PAGE_SIZE`, `MAX_PAGE_SIZE`, or `REPORTS_PAGE_SIZE` when a screen genuinely needs its own), not by number (`PAGE_25`).
- **Environment-dependent values** (URLs, keys, limits that differ per deploy) belong in config or env variables, not in constants.
- **Follow the project's layout and naming.** Use its existing `helpers/`, `constants/` or `config/` folder and its casing convention (`UPPER_SNAKE_CASE` is the usual default for constants; match the repo if it differs). Create a new shared folder only if none exists. Split by domain (`pagination.ts`, `routes.ts`) and re-export from `index` so imports stay short.
- The same idea applies to CSS: colors, spacing, radii and font sizes are design tokens (custom properties), not repeated literals.

See `references/kiss-dry-scalability.md` for more worked examples, refactors, and how to balance these rules against each other.

## HTML, CSS and JavaScript

- Use the element that matches the meaning: `button` for actions, `a href` for navigation, `label` tied to every form control, headings in order, landmarks (`header`, `nav`, `main`, `footer`), `ul`/`ol` for lists, `table` only for tabular data. Avoid `div` soup.
- Check content-model rules and attributes against the HTML spec rather than from memory when it matters (for example what an `a` or `p` may contain).
- Prefer native platform features over libraries and custom widgets.
- In JS, prefer `async`/`await` over callbacks, always handle rejections, and run independent work with `Promise.all` or `Promise.allSettled`.

Details and a longer checklist are in `references/web-platform.md`.

## Design patterns

Start from the problem, not the pattern. Identify the pain (duplicated conditionals, tight coupling, object creation scattered everywhere), then pick the pattern that addresses it, and use the simplest form of it. If a plain function solves it, use the function. `references/design-patterns.md` maps common problems to patterns and links them to patterns.dev and refactoring.guru.

## Using the reference sources

`references/sources.md` explains what each source is good for, how its URLs are structured, and how to fetch the right page cheaply. Read it when you need to verify a spec detail, pick or implement a pattern, or recognize a code smell. Fetch one targeted page, take what you need, and move on.

## Checklist before handing work back

- [ ] I searched for existing helpers/components and reused them where they fit.
- [ ] No meaningful literal is repeated; shared values live in one module and are imported.
- [ ] The solution is the simplest one that works; every abstraction has a named reason.
- [ ] Duplicated logic that encodes the same rule is unified; look-alike code with different reasons to change is left alone.
- [ ] HTML is semantic, forms are labelled, and nothing contradicts the spec.
- [ ] Any design pattern used solves a concrete problem I can state in one sentence.
- [ ] Naming and folder placement follow the project's existing conventions.
