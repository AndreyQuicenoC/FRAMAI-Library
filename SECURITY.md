# Security Policy

## Supported versions

| Version | Supported |
| --- | --- |
| 3.0.x | Yes |
| 2.2.x | Yes |
| < 2.2 | No |

## Reporting a vulnerability

If you discover a security issue in VEXARQ prompts, templates, or documentation:

1. Do **not** open a public issue for sensitive findings.
2. Contact the maintainer via GitHub: [AndreyQuicenoC](https://github.com/AndreyQuicenoC).
3. Include steps to reproduce, affected files, and potential impact.

We aim to acknowledge reports within 7 days.

## Scope note

This repository contains AI prompt assets, not runtime application code. Reports related to downstream projects that consume `.ai-vexarq/` should be directed to those projects when the issue is in generated application code rather than in the framework itself.

Framework rules that agents must keep:

- Do not push unless the user names the remote and the branch.
- Do not commit secrets or edit files outside the requested scope.
- Do not treat a preliminary artifact as the input of the next phase.
