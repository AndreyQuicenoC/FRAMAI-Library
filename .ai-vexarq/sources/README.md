# VEXARQ sources

## Canon

These files are executed in full. They are not rewritten, summarized, or adapted inside the file.

| File | Phase |
|---|---|
| `canon/backlog-hu-generator.md` | Preliminary backlog: generate and validate user stories |
| `canon/archetype-prompt.xml` | Archetype: analyze the current SAD and generate the skeleton |

Each phase workflow says how to invoke them and where to write the result. That wrapper does not replace the canon.

## External example

`examples/backlog-extendido-proyecto-externo.md` is the prompt that produced the extended backlog of a project outside this library. It calibrates generation. It is not a universal template: it contains that project's acronym, sprints, roles, and counts.

The generator to use on any project is `workflows/lifecycle/backlog-extended.xml`.
