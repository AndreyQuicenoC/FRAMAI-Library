# .ai-vexarq

Portable VEXARQ folder. Copy it to a project root and paste a template from `templates/lifecycle/`.

The instructions in this folder are in English. Generated product text follows `.ai-vexarq/core/language.xml`: the user's language, or the language the user names.

Guide: [guides/usar-vexarq.md](guides/usar-vexarq.md).

## Entry points

| File | Use |
|---|---|
| [workflows/lifecycle/orchestrator.xml](workflows/lifecycle/orchestrator.xml) | Detects the phase or walks the cycle |
| [core/language.xml](core/language.xml) | Selects the language of generated text and code comments |
| [templates/lifecycle/ciclo.md](templates/lifecycle/ciclo.md) | Text to paste, validar mode |
| [templates/lifecycle/fase.md](templates/lifecycle/fase.md) | A single phase |
| [templates/lifecycle/continuo.md](templates/lifecycle/continuo.md) | Start to finish, with visible assumptions |
| [workflows/user-story-implementation/orchestrator.xml](workflows/user-story-implementation/orchestrator.xml) | Implement a user story when a base already exists |

## Lineage rule

`docs/vexarq/preliminary/` is the model's output. `docs/vexarq/current/` is what the next phase may read. The board is `docs/vexarq/status.html`, created from [tracker/project-status.template.html](tracker/project-status.template.html).

## Canon

Do not edit [sources/canon/](sources/canon/). The example from another project is in [sources/examples/](sources/examples/) and does not apply outside that product.

## Story implementation

The adaptive engine (orchestrator, backend, frontend, integrator, complexity profiles, and standards) is kept. Its operating guide remains [guides/user-story-implementation.xml](guides/user-story-implementation.xml).

Version 3.0.0.
