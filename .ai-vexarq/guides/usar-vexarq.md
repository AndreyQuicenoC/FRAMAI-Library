# How to use VEXARQ

VEXARQ is the `.ai-vexarq` folder. Copy it whole to the project root and use it from the agent chat. Nothing needs to be installed.

The library itself is written in English. Generated documents, interface text, and comments follow the language of the user's request. If the user names a language, that language wins. The rule is `.ai-vexarq/core/language.xml`.

## 1. Copy

Copy `.ai-vexarq/` to the root of the repository you will work in. Optional: copy `.ai-vexarq/cursor/vexarq.mdc` to `.cursor/rules/vexarq.mdc` so the editor keeps the rules.

## 2. Choose how to start

| Intention | What to paste |
|---|---|
| Walk the cycle and let the system detect the phase | `templates/lifecycle/ciclo.md` |
| Work on a single phase | `templates/lifecycle/fase.md` |
| Build from start to finish without stopping at each review | `templates/lifecycle/continuo.md` |
| Implement a story when code or an archetype already exists | `templates/user-story-implementation/` |

The file the agent must follow is `workflows/lifecycle/orchestrator.xml`. The templates only invoke it.

## 3. Modes

`validar` is the default. The model writes to `docs/vexarq/preliminary/` and stops. You edit and leave the accepted version in `docs/vexarq/current/`. The next phase reads only `current/`.

`continuo` fills gaps from the library's formats and examples, marks each gap as an assumption, and continues through the requested scope: documentation, frontend, backend, and tests, according to the product. It does not authorize a push.

## 4. Phase order

1. Discovery, including the business model.
2. SRS.
3. Database.
4. Backlog, with the canonical generator left intact.
5. Extended backlog as CSV.
6. SAD.
7. Archetype, with the canonical generator left intact.
8. Implementation, according to the real structure: frontend, backend, data, tests, and documentation.
9. Delivery.
10. Recommendations for software, technical work, documentation, the business, and future capabilities.

A small correction does not walk that list.

## 5. Board

`docs/vexarq/status.html` is created from `tracker/project-status.template.html` and updated at the end of each request. It shows the phase, the state, and a link. The contract is `formats/tracker/STATUS.md`.

## 6. What not to touch

- `sources/canon/`: executed as written.
- A current document you already edited: the model does not overwrite it.
- Files outside the scope, behavior you did not ask to change, secrets, and unauthorized pushes.

## 7. Where to read next

- Phases and gates: `core/approval-gates.xml`
- Preliminary and current documents: `core/artifact-lineage.xml`
- Language of generated text: `core/language.xml`
- Extended backlog: `workflows/lifecycle/backlog-extended.xml` and `formats/backlog/EXTENDED_BACKLOG.md`
- User stories on existing code: `guides/user-story-implementation.xml`
- Git: `core/git-workflow.xml` and `formats/delivery/COMMIT_CONVENTIONS.md`
