# dev-standards

Skill para agentes de desarrollo que impone **KISS**, **DRY** y **escalabilidad** al escribir código web, y que los orienta a las fuentes correctas (spec de HTML, DevDocs, patterns.dev, refactoring.guru) cuando tienen dudas.

El contenido de la skill (`SKILL.md` y referencias) está escrito en inglés porque suele activar mejor a los modelos. Este README está en español.

## Qué resuelve

- **Nada de valores mágicos.** En vez de repetir `const page_size = 25;` en cada archivo, el agente crea (o reutiliza) un módulo compartido, por ejemplo `helpers/index.ts` con `export const DEFAULT_PAGE_SIZE = 25;`, y lo importa en todo el sitio.
- **Reutilizar antes de crear.** Busca helpers, constantes y componentes existentes antes de escribir código nuevo.
- **Simple primero.** Cada abstracción o patrón tiene que justificarse con un problema concreto. DRY se aplica al conocimiento duplicado, no al texto parecido.
- **HTML, CSS y JS correctos.** Semántica, formularios accesibles, tokens de diseño en CSS y buenas prácticas de código asíncrono.
- **Patrones con criterio.** Parte del problema, no del patrón, y enlaza a patterns.dev y refactoring.guru.

## Estructura del repo

```
.
├── README.md
├── LICENSE
└── skills/
    └── dev-standards/
        ├── SKILL.md                        # flujo, principios y checklist (corto)
        └── references/
            ├── sources.md                  # cómo consultar las 4 fuentes + mapa de URLs
            ├── kiss-dry-scalability.md     # ejemplos, estructura de carpetas, cuándo NO aplicar DRY
            ├── design-patterns.md          # problema → patrón, code smells → refactor
            └── web-platform.md             # HTML, CSS y JS asíncrono
```

La skill usa *divulgación progresiva*: `SKILL.md` se carga siempre que la skill se activa y los archivos de `references/` se leen solo cuando hacen falta. Así el contexto del agente se mantiene liviano.

## Instalación

### Claude Code, a nivel proyecto (se comparte por git con el equipo)

```bash
git clone https://github.com/<tu-usuario>/<repo>.git
mkdir -p /ruta/al/proyecto/.claude/skills
cp -r <repo>/skills/dev-standards /ruta/al/proyecto/.claude/skills/
```

Después commiteá `.claude/skills/dev-standards` en ese proyecto.

### Claude Code, a nivel personal (todos tus proyectos)

```bash
cp -r skills/dev-standards ~/.claude/skills/
```

Si el directorio `~/.claude/skills/` no existía cuando abriste la sesión, reiniciá Claude Code para que lo detecte. Corré `/skills` para verificar que la skill aparece.

### Claude.ai

Comprimí la carpeta `dev-standards` en un ZIP y subilo desde la configuración de Skills. Las actualizaciones son manuales: hay que volver a subir el ZIP.

## Usarla con subagentes

Los subagentes de Claude Code no heredan las skills de la sesión principal. Para que un agente la tenga, listala en su frontmatter y se precarga al iniciar:

```markdown
---
name: frontend-dev
description: Implementa features de frontend siguiendo los estándares del equipo
skills:
  - dev-standards
---

Instrucciones propias del agente...
```

## Fuentes que consulta

| Fuente | Para qué |
|---|---|
| [WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/) | Verificar elementos, atributos y modelos de contenido |
| [DevDocs](https://devdocs.io/) | Documentación de APIs por tecnología |
| [patterns.dev](https://www.patterns.dev/) | Patrones de JavaScript, React y Vue |
| [refactoring.guru (es)](https://refactoring.guru/es) | Patrones clásicos y code smells |
| [MDN](https://developer.mozilla.org/) (adición opcional) | Respaldo legible cuando DevDocs no renderiza |

Notas:

- La skill **no copia** esos sitios: indica cuándo y qué página puntual consultar. Los agentes necesitan acceso a internet para hacerlo.
- `https://devdocs.io/async/` corresponde a la librería `async` de JavaScript, no a JavaScript en general. Para HTML, CSS y JS se usan otros slugs de DevDocs (por ejemplo `html`, `css`, `javascript`; confirmalos en el índice de DevDocs).
- DevDocs se renderiza con JavaScript y un fetch simple puede traer solo el menú. Por eso MDN figura como respaldo.

## Cómo mantenerla y actualizarla

1. Editá en una rama (`git switch -c mejora/<tema>`).
2. Respetá la forma de la skill:
   - `SKILL.md` corto (idealmente menos de 500 líneas). Lo detallado va en `references/`.
   - Cada referencia nueva se menciona en `SKILL.md` con una indicación de **cuándo** leerla.
   - Explicá el *porqué* de cada regla; funciona mejor que los "SIEMPRE" y "NUNCA".
   - El campo `description` decide cuándo se activa la skill. Si el agente no la usa cuando debería, ahí es donde hay que ajustar.
3. Probala con tareas reales (un paginador, un formulario, un refactor) comparando con y sin la skill.
4. Abrí un pull request y, al mergear, etiquetá una versión (`git tag v1.1.0 && git push --tags`).
5. En los proyectos que la usan, traé los cambios y volvé a copiar la carpeta (o actualizá el ZIP en Claude.ai).

Cuándo conviene tocar qué:

| Quiero... | Edito |
|---|---|
| Agregar o cambiar una regla de código | `SKILL.md` o `references/kiss-dry-scalability.md` |
| Sumar una fuente de consulta | `references/sources.md` |
| Ajustar recomendaciones de patrones | `references/design-patterns.md` |
| Reglas de HTML, CSS o JS | `references/web-platform.md` |
| Que se active más (o menos) | `description` en el frontmatter de `SKILL.md` |

## Licencia

[MIT](./LICENSE). La licencia cubre el contenido de este repo. Los sitios enlazados (WHATWG, DevDocs, patterns.dev, refactoring.guru, MDN) pertenecen a sus respectivos autores y se rigen por sus propios términos; la skill solo enlaza a ellos y no reproduce su contenido.
