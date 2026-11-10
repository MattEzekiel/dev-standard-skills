# Reference sources

Contents:
1. How to consult sources (read this first)
2. WHATWG HTML Living Standard
3. DevDocs
4. patterns.dev
5. refactoring.guru (Spanish)
6. Optional addition: MDN

## 1. How to consult sources

- These are sites to look things up in, not material to memorize. Fetch **one targeted page** for the question at hand.
- Summarize in your own words. Do not paste large passages of these sites into code, comments or docs; link to them instead.
- If a page cannot be fetched (network restrictions, site rendered with JavaScript), say so and fall back to the next source or to what you know, flagging the uncertainty.

## 2. WHATWG HTML Living Standard

Base: `https://html.spec.whatwg.org/multipage/`

Use it to verify: which elements and attributes exist, where an element is allowed, its content model, and required behavior. It is the authority; tutorials and blog posts are not.

Fetch the multipage chapter you need, never the single-page version (`https://html.spec.whatwg.org/`), which is several megabytes.

| Topic | Chapter URL |
|---|---|
| Introduction | `/multipage/introduction.html` |
| Common infrastructure | `/multipage/infrastructure.html` |
| Documents, DOM | `/multipage/dom.html` |
| The elements of HTML (overview) | `/multipage/semantics.html` |
| Sections (`header`, `nav`, `main`, `article`, `section`, headings) | `/multipage/sections.html` |
| Text-level semantics (`a`, `em`, `strong`, `code`, `time`, ...) | `/multipage/text-level-semantics.html` |
| Edits (`ins`, `del`) | `/multipage/edits.html` |
| Embedded content (`img`, `picture`, `video`, `iframe`, ...) | `/multipage/embedded-content.html` |
| Tabular data (`table`, `th`, `scope`, ...) | `/multipage/tables.html` |
| Forms (`form`, `input`, `label`, `select`, validation) | `/multipage/forms.html` |
| User interaction (`hidden`, `inert`, focus, editing, drag and drop) | `/multipage/interaction.html` |
| Microdata | `/multipage/microdata.html` |
| Loading web pages | `/multipage/browsers.html` |

To jump to one element, append its anchor, for example `/multipage/forms.html#the-input-element` or `/multipage/sections.html#the-main-element`. Chapters beyond these (web application APIs, workers, web storage, and so on) exist in the table of contents at `https://html.spec.whatwg.org/multipage/`; read the index to find the exact path.

Tip: to check a markup question quickly, read the element's "Categories", "Contexts in which this element can be used", "Content model" and "Content attributes" blocks. Those four answer most questions.

## 3. DevDocs

Site: `https://devdocs.io/`. It aggregates API documentation for many languages and libraries in one searchable place.

Things to know:

- **`https://devdocs.io/async/` is the documentation of the Async library for JavaScript** (callback and control-flow utilities such as `parallel`, `series`, `queue`), version 3.x. It is not general JavaScript documentation. Use it only when the project already depends on the `async` package. For new code, prefer native `async`/`await` and `Promise` APIs.
- Each technology has its own slug in the form `https://devdocs.io/<slug>/`. Typical slugs are `html`, `css`, `javascript`, `dom`, `typescript`, `node`, `http`; others exist for frameworks and libraries. Confirm a slug exists in the DevDocs index before relying on it.
- DevDocs renders its content with JavaScript, so a plain page fetch often returns only the navigation shell and no documentation text. If that happens, use MDN (section 6) for HTML, CSS, JavaScript and Web APIs, which is the same underlying material for those topics.

## 4. patterns.dev

Base: `https://www.patterns.dev`

Organized by ecosystem. It covers design, rendering and performance patterns with JavaScript and React examples. Use it to decide on and implement a pattern in a JS/React/Vue codebase.

**JavaScript (vanilla)** under `/vanilla/`
- Design patterns: `singleton-pattern`, `proxy-pattern`, `prototype-pattern`, `observer-pattern`, `module-pattern`, `mixin-pattern`, `mediator-pattern`, `flyweight-pattern`, `factory-pattern`
- Loading and performance: `static-import`, `dynamic-import`, `import-on-visibility`, `import-on-interaction`, `route-based`, `bundle-splitting`, `prpl`, `tree-shaking`, `preload`, `prefetch`, `third-party`, `virtual-lists`, `compression`, `loading-sequence`, `view-transitions`

**React** under `/react/`
- Component patterns: `presentational-container-pattern`, `hoc-pattern`, `render-props-pattern`, `hooks-pattern`, `compound-pattern`
- Rendering: `client-side-rendering`, `server-side-rendering`, `static-rendering`, `incremental-static-rendering`, `progressive-hydration`, `streaming-ssr`, `react-server-components`
- Other: `nextjs`, `nextjs-vitals`, `react-2026`

**Vue** under `/vue/`
- `components`, `async-components`, `composables`, `container-presentational`, `data-provider`, `dynamic-components`, `provide-inject`, `render-functions`, `renderless-components`, `script-setup`, `state-management`

Example full URL: `https://www.patterns.dev/vanilla/observer-pattern`

## 5. refactoring.guru (Spanish)

Base: `https://refactoring.guru/es`

Language-agnostic explanations of the classic design patterns and of code smells. Best for understanding *why* a pattern exists, its structure, and when it applies or backfires.

- Design patterns: `/es/design-patterns`
  - Creational: `/es/design-patterns/creational-patterns` (Factory Method, Abstract Factory, Builder, Prototype, Singleton)
  - Structural: `/es/design-patterns/structural-patterns` (Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy)
  - Behavioral: `/es/design-patterns/behavioral-patterns` (Chain of Responsibility, Command, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor)
- Refactoring: `/es/refactoring` (the navigation marks it as coming soon, so some pages may be incomplete)
  - Smells: `/es/refactoring/smells` with groups `bloaters`, `oo-abusers`, `change-preventers`, `dispensables`, `couplers`, `other`
  - Techniques: `/es/refactoring/techniques`
  - Catalog: `/es/refactoring/catalog`

Individual pattern pages follow the form `/es/design-patterns/<pattern-slug>`, for example `/es/design-patterns/strategy`. If a guessed slug returns an error, open the category page above and follow the link.

The site's book and courses are paid content. Link to them; do not reproduce them.

## 6. Optional addition: MDN

`https://developer.mozilla.org/` is not in the original list but is worth keeping as a fallback. It documents HTML elements and attributes, CSS properties and JavaScript/Web APIs in readable, server-rendered pages, with browser compatibility tables. Use it when DevDocs does not render and for practical usage detail. For normative questions (what the standard requires), the WHATWG spec still wins.
