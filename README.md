# VEXARQ

A modular system for AI-assisted software engineering. Copy the `.ai-vexarq` folder into a project and the agent has a way to interpret a request, decide which artifact to produce, decide what a person must validate, and decide which evidence closes the work.

VEXARQ continues FRAMAI. The multi-agent user-story engine, the engineering standards, and the Git flow from version 2.2.0 remain in the library. Version 3.0.0 puts a product lifecycle around that engine.

The library is written in English. Code, documents, and other generated text follow the language the user is using. An explicit language request overrides that detection. See `.ai-vexarq/core/language.xml`.

## The problem it addresses

A bundle of loose prompts produces documents that do not talk to each other. VEXARQ separates the engine, the templates, the output contracts, the current context, and the check. The next phase cannot take a model draft that is not yet the current document.

## How it differs

| | Spec Kit | BMAD | VEXARQ |
|---|---|---|---|
| Unit of work | A feature specification, with a command per phase | A team of agents and handoffs between files | A product cycle and, inside it, the user-story engine already proven in FRAMAI |
| Authority of a requirement | The kit's constitution and specification | The previous agent's handoff | The current document a person edited. The model's preliminary does not feed the next phase |
| Backlog and archetype | Tasks derived from the spec | Planning by role | Canonical generators of its own, executed without rewriting them, plus a project-agnostic extended backlog in CSV |
| Close | Convergence of the feature | Review by the quality agent | A one-page HTML board, a request report, and recommendations for the product, the technical work, the documentation, the business, and future work |
| Publishing | Depends on the host agent | Depends on the host agent | Push is forbidden unless the remote, the branch, and explicit authorization are present, with checks green |

Spec Kit and BMAD are references for specification-driven development. VEXARQ does not wrap them. It fixes preliminary and current documents, keeps the generators that already worked, and requires evidence before a phase is called done.

## How to use it

1. Copy `.ai-vexarq/` to the project root.
2. Open `.ai-vexarq/templates/lifecycle/ciclo.md`, `fase.md`, or `continuo.md`.
3. Paste the template into the agent and write the request, in the language you want the product artifacts to use.

Full guide: [`.ai-vexarq/guides/usar-vexarq.md`](.ai-vexarq/guides/usar-vexarq.md).

`validar` is the default mode: the model leaves the result in `docs/vexarq/preliminary/` and waits. `continuo` fills gaps with visible assumptions and can walk documentation, frontend, backend, and tests. Neither mode pushes on its own.

## Phases

Discovery, SRS, database, backlog, extended backlog, SAD, archetype, implementation, delivery, and recommendations. The board `docs/vexarq/status.html` shows which phase the project is in, what is missing, and the link to the document. The empty template is `.ai-vexarq/tracker/project-status.template.html`.

Discovery identifies the business model (SaaS, marketplace, commerce, internal tool, API, content, on-premise, institutional, or other). It does not assume SaaS.

## What each format holds

- XML: prompts, policies, and registries.
- Markdown: document contracts.
- CSV: the extended backlog.
- Static HTML: the phase board, rewritten on each request without turning it into a report.

The backlog and archetype generators live in `.ai-vexarq/sources/canon/` and are executed in full. An extended-backlog prompt from an external project stays in `sources/examples/` as calibration. The universal generator is `workflows/lifecycle/backlog-extended.xml`.

## Working rules

- Do not edit files outside the scope.
- Do not change functional behavior the request did not ask to change.
- Do not commit secrets.
- Do not push without explicit authorization of the destination.
- Do not report a test as passed if it was not run.

Detail: [SECURITY.md](SECURITY.md) and `.ai-vexarq/core/git-workflow.xml`.

## Folder map

```text
.ai-vexarq/
  workflows/lifecycle/          one phase template and the general orchestrator
  workflows/user-story-implementation/   FRAMAI user-story engine
  sources/canon/                backlog and archetype, executed as written
  sources/examples/             external extended-backlog example
  formats/                      Markdown contracts
  tracker/                      base HTML for project status
  core/                         policies, modes, lineage, language, git, design
  guides/usar-vexarq.md         usage guide
  templates/lifecycle/          text ready to paste
```

## License

MIT. See [LICENSE](LICENSE).
