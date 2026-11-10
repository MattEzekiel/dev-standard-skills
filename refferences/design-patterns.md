# Design patterns: choosing and using them

Contents:
1. Principle: problem first
2. Problem to pattern map
3. Patterns specific to frontend frameworks
4. Code smells and the refactoring that fixes them
5. When not to use a pattern

## 1. Principle: problem first

A pattern is a named solution to a recurring problem. Using one without the problem adds code and indirection for nothing. Before applying a pattern, state in one sentence what hurts today (for example, "adding a new payment method requires editing five `if` chains"). If you cannot, do not use a pattern.

Prefer the lightest form: a function or an object literal often does what a class hierarchy would.

For definitions, structure and caveats, fetch the pattern's page from refactoring.guru (language-agnostic) or patterns.dev (JavaScript/React/Vue). URL structure is in `references/sources.md`.

## 2. Problem to pattern map

| What hurts | Pattern to consider | Light form in JS/TS |
|---|---|---|
| Long `if/else` or `switch` choosing between interchangeable algorithms | Strategy | A map of functions: `const handlers = { card: payCard, paypal: payPaypal }` |
| Object creation logic scattered and varying by type | Factory Method / Abstract Factory | A `createX(type)` function |
| Constructing objects with many optional parts | Builder | An options object with defaults |
| One part of the app must react to changes in another without tight coupling | Observer | Event emitter, `EventTarget`, signals |
| Incompatible interface from a library or legacy API | Adapter | A small wrapper function mapping shapes |
| Complex subsystem with a noisy API | Facade | One module exposing 2 or 3 simple functions |
| Add behavior to an object without subclassing | Decorator | Function wrapping a function (middleware, HOC) |
| Control access, add caching or lazy loading to an object | Proxy | JS `Proxy`, or a caching wrapper |
| Behavior depends on state and the state graph is growing | State | Explicit state machine (object of transitions) |
| Undo/redo, queued or logged operations | Command | Plain objects `{ type, payload }` plus handlers |
| Request passes through steps that may stop or transform it | Chain of Responsibility / Mediator-Middleware | Middleware array |
| A skeleton algorithm with customizable steps | Template Method | Function that accepts step callbacks |
| One shared instance truly needed | Singleton | A module-level export (ES modules are already singletons). Use sparingly; it hides dependencies and complicates tests |
| Many similar objects eating memory | Flyweight | Share the immutable data between instances |
| Grouping behavior and state with private internals | Module | ES module with unexported internals |

## 3. Patterns specific to frontend frameworks

Use these when the codebase uses the framework in question.

- **React:** Container/Presentational (split data fetching from rendering), Hooks (share stateful logic), Compound components (flexible related components such as tabs), Render props and HOC (older approaches, hooks usually replace them).
- **Vue:** Composables (share stateful logic), Provide/Inject (pass data down without prop drilling), Renderless components, Data Provider.
- **Loading and performance:** Dynamic import and route-based splitting for large bundles, list virtualization for very long lists, preload/prefetch for critical resources.
- **Rendering strategy (React/Next.js):** choose between client-side, server-side, static and incremental rendering based on how often the data changes and how important first paint and SEO are.

## 4. Code smells and the refactoring that fixes them

Smells are symptoms that a refactor is due. refactoring.guru groups them; these are the ones that come up most.

| Smell (group) | What it looks like | Typical fix |
|---|---|---|
| Magic number (Dispensables) | Bare literals with hidden meaning | Replace with a named constant in a shared module |
| Duplicate code (Dispensables) | Same logic in several places | Extract function or module; unify |
| Long method (Bloaters) | A function doing many things | Extract Method |
| Large class (Bloaters) | A class or component with too many responsibilities | Extract Class or split the component |
| Long parameter list (Bloaters) | Many positional arguments | Introduce a parameter object |
| Primitive obsession (Bloaters) | Strings and numbers standing in for concepts | Introduce a small type or constant set |
| Shotgun surgery (Change preventers) | One change needs edits in many files | Gather the scattered logic in one place |
| Divergent change (Change preventers) | One file changes for many unrelated reasons | Split by responsibility |
| Switch statements on type (OO abusers) | Repeated conditionals over a kind | Strategy or polymorphism, or a lookup map |
| Speculative generality (Dispensables) | Hooks, options and layers nobody uses | Delete them (KISS) |
| Feature envy / inappropriate intimacy (Couplers) | Code reaching deep into another module's data | Move the behavior to where the data lives |

## 5. When not to use a pattern

- The problem occurs once and is unlikely to repeat.
- A function, a map or a plain object solves it in fewer lines.
- The team does not know the pattern and the gain is small. Clarity beats cleverness.
- You are adding it "for flexibility" with no concrete second case.

If a pattern is justified, mention its name in a short comment or the PR description so the next reader can look it up.
