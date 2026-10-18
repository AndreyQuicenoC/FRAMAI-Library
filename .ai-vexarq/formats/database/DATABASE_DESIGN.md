# Data design

## 1. Decision

Relational, document, mixed, or the engine the repository already uses. Reason, and the rejected alternative when there was a real choice.

## 2. Scope

Requirements and stories this model serves. What it deliberately does not persist.

## 3. Conceptual model

Entities or aggregates, attributes, and relationships in domain language. A Mermaid diagram.

## 4. Logical model

Keys, cardinalities, uniqueness, and constraints. In a document model: collections, references, and embedded documents, with the reason for each embedding.

## 5. Integrity and lifecycle

Creation, update, and deletion. Logical or physical deletion, retention, and audit when they apply. Personal data is marked.

## 6. Physical model

Types, nullability, defaults, and indexes with the query they serve. Partitions or replicas only when the profile or the SRS asks for them.

## 7. Access

The operations the product needs and how the model answers them. An index without a query is not added.

## 8. Migrations

Versioned changes from the current schema. If the project is starting, the initial migration. Do not execute it from this document.

## 9. Security

Least privilege, sensitive data, and what must not appear in logs.

## 10. Open decisions

Decisions the SAD must not treat as closed.
