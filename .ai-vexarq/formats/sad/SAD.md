# Software architecture document

## 1. Purpose and scope

Which requirements this architecture serves and which it leaves for later.

## 2. Context

Users, the system, and external systems. A C4 context diagram in Mermaid.

## 3. Containers

Applications, data stores, and interfaces. The technology of each one and the reason it was chosen. A Mermaid diagram.

## 4. Components

Only when there is more than one module with its own responsibility. Otherwise, write that the container level is enough.

## 5. Decisions

One subsection per decision that is hard to reverse:

- Context.
- Options.
- Decision.
- Consequences.

## 6. Data

How the current model is used. Allowed divergences are proposals, not silent edits.

## 7. Interfaces

API or event contracts that are in scope: operations, errors, and compatibility.

## 8. Quality

Measurable scenarios for the characteristics the SRS selected.

## 9. Security

Trust boundaries, authentication, authorization, secrets, and data in transit and at rest, at the level the product needs.

## 10. Project structure

Directories and modules the archetype must respect. If the repository already has a structure, this section documents it and marks only the changes.

## 11. Deployment and operation

Environments, configuration, and the minimum needed to run the system. Do not invent a production cluster.

## 12. Risks

Risk, impact, and the decision still open.
