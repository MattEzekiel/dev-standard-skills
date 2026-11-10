# HTML, CSS and JavaScript practices

Contents:
1. HTML
2. CSS
3. JavaScript and async code
4. Verifying with the sources

## 1. HTML

**Choose elements by meaning.**

- Actions use `<button type="button">` (or `type="submit"` in a form). Navigation uses `<a href>`. Do not attach click handlers to `div` or `span` to fake either.
- Structure pages with landmarks: one `<main>`, plus `<header>`, `<nav>`, `<footer>` and `<aside>` where they apply. Use `<section>` when it has a heading, and `<article>` for self-contained content.
- Headings follow a logical outline (do not skip levels for styling; style with CSS instead).
- Lists use `<ul>`, `<ol>`, `<dl>`. Tables are for tabular data, with `<th>` and `scope` set.
- Use `<time>`, `<address>`, `<figure>`/`<figcaption>`, `<details>`/`<summary>`, `<dialog>` when they fit, instead of rebuilding them.

**Forms.**

- Every control has an associated `<label>` (`for`/`id` or wrapping).
- Use the right `type` (`email`, `tel`, `number`, `date`, ...) and the built-in validation attributes (`required`, `min`, `max`, `pattern`) before writing JS validation.
- Group related controls with `<fieldset>` and `<legend>`.
- A `<button>` inside a form defaults to submit; set `type="button"` when it should not submit.

**Media.**

- Every `<img>` has `alt` (empty `alt=""` for purely decorative images). Give `width` and `height` to prevent layout shift. Use `loading="lazy"` for below-the-fold images.

**Accessibility.**

- First rule of ARIA: if a native element does the job, use it and skip ARIA. Add ARIA only for gaps native HTML cannot cover.
- Everything interactive must work with the keyboard and show a visible focus state.

**Validity.** Mind content models: for example, interactive elements cannot be nested inside `<a>` or `<button>`, and `<p>` only accepts phrasing content. When unsure, check the element's content model in the spec (see `references/sources.md`) and validate the markup with the Nu HTML checker.

## 2. CSS

- Put design values in custom properties (`--space-*`, `--color-*`, `--radius-*`, `--font-size-*`). This is the DRY rule applied to styling.
- Keep specificity low and consistent: classes over IDs, avoid `!important`, avoid deep selector chains.
- Use one naming convention for classes across the project (BEM, utility classes, CSS modules) and follow the existing one.
- Build mobile-first with `min-width` media queries or container queries. Define breakpoints once (custom properties cannot be used inside media queries, so keep them in one documented place such as a shared constants file or the preprocessor config).
- Prefer Flexbox and Grid over floats or positioning hacks. Use `rem` for type and spacing so user font settings are respected.
- Avoid magic numbers: if a value is derived (for example a header height used elsewhere), name it.

## 3. JavaScript and async code

- Prefer `async`/`await` over nested callbacks and manual `.then` chains; wrap awaits in `try/catch` where failure is expected, and never leave a promise unhandled.
- Run independent async work concurrently with `Promise.all`. When one failure must not discard the others, use `Promise.allSettled`.
- Use `AbortController` to cancel stale requests (search-as-you-type, route changes) and to enforce timeouts.
- Do not mix sequential `await` in a loop when the calls are independent; that serializes work needlessly.
- Prefer native `fetch`, `URL`, `URLSearchParams`, `structuredClone`, `Intl` over adding a library for the same thing.
- The `async` package (documented at `https://devdocs.io/async/`) is callback-oriented control-flow tooling. Use it only if the project already depends on it; otherwise native promises cover the same needs.
- Prefer `const`, small pure functions and early returns. Avoid deeply nested conditionals.
- Validate input at boundaries (user input, network responses) and keep error messages in the shared constants module.

## 4. Verifying with the sources

| Question | Where to look |
|---|---|
| Is this element or attribute valid here? What does it require? | WHATWG HTML spec chapter for the element |
| How does a CSS property or JS API behave, and who supports it? | MDN (or DevDocs `css` / `javascript` / `dom` if it renders) |
| Is there a standard pattern for this problem? | `references/design-patterns.md`, then patterns.dev or refactoring.guru |
| Is this a known code smell? | refactoring.guru smells section |

URLs and structure for each are in `references/sources.md`.
