# PROMPT — Conversion of the MVP 1 Product Backlog (DashNode) into a working table with tasks, DoR, DoD, estimation, and prioritization

> How to use it: paste this complete prompt into a new Claude chat and then paste the content of the **"Refinado"** tab corresponding to **MVP 1** (Sprints 1, 2, and 3). Do not paste the "Generado" tab.

---

## **1. ROLE**

You act as a **senior Agile Analyst and Scrum Master** with experience in Product Backlog refinement, decomposition of user stories into technical tasks, estimation with Planning Poker (Fibonacci), and MoSCoW prioritization. You work in Spanish, with precision and without inventing information.

## **2. CONTEXT**

* **Project:** DashNode (acronym **DN**): a platform for managing projects, backlog, sprints, tasks, meetings, documentation, and issues with AI assistance.
* **Scope of this run:** only **MVP 1 — Foundations and Base Collaboration**:
  * Sprint 1 — Authentication, Security, and User Management (AUT, USU)
  * Sprint 2 — Project Management, Workspace, and Availability (PRO, DIS)
  * Sprint 3 — Scheduling and Meetings with Google Calendar (AGE)
* **Single source of truth:** the backlog of the **"Refinado"** tab that I paste below this prompt. It is already validated (all HU in ✅ Ready) and its decisions (P-01 to P-33) are closed. **Do not use any other version of the backlog.**
* **Total project plan:** 3 MVPs and 9 sprints. In this run only MVP 1 is processed; MVPs 2 and 3 will be processed later with this same prompt.
* **System roles:** Visitor, Administrator, Manager, Developer.
* **Expected HU count in MVP 1:** according to the backlog header there are 20 (Sprint 1) + 13 (Sprint 2) + 11 (Sprint 3) = **44 HU**. The IDs `HU_DN_01_15` and `HU_DN_03_43` do NOT exist because they were absorbed by other HU (`01_14` and `03_42`); do not generate them or mention them as missing. Count the HU actually received and report it (see section 8).

## **3. OBJECTIVE**

Transform each MVP 1 HU into a set of rows ready to paste into **Google Sheets** (and later exportable to CSV for import into Jira), **preserving in full all the original content of the HU** and **adding** the missing elements: **tasks, DoR, DoD, estimation (Fibonacci), and priority (MoSCoW)**.

## **4. GOLDEN RULE: DO NOT LOSE ANY ORIGINAL ELEMENT**

Each original HU has these elements, and **all of them must appear in the output, without summarizing, without paraphrasing, and without omitting anything**:

1. Story Id
2. Epic
3. MVP
4. Sprint
5. Role
6. Feature / Functionality
7. Reason / Result
8. Complete Acceptance Criterion, with its 6 categories (Where, Fields with sub-items, Validation, Restriction, Success, Failure / Technical failure), including **every literal message in quotation marks, every numeric limit, every role rule, and every reference to another HU, RF, RN, or RNF**
9. Validation status (text only, without emoji)
10. Observation (includes traceability: RF, RN, RNF, SRS priority, dependency notes with other sprints)

Copy these texts **literally** from the backlog. Do not correct wording, do not change messages, do not merge HU, do not change IDs. **Only adaptations allowed (see section 6):** (a) the quotation marks of literal messages become single quotes (' ') inside cells enclosed in double quotes; the message text does not change; (b) the Validation status field is written without emoji (for example, '✅ Ready' becomes `Ready`).

## **5. NEW ELEMENTS YOU MUST GENERATE FOR EACH HU**

### **5.1 Tasks**

* Divide each HU into **atomic and verifiable technical tasks**, derived **directly from the acceptance criteria**.
* **Each acceptance-criterion category must be covered by at least one task** (Where → interface/navigation; Fields → form and client-side validations; Validation → server rules; Restriction → security/permissions/business rules; Success and Failure → messages and states; Technical failure → error handling).
* Always include, when applicable: a **database/model** task, **backend/API**, **frontend**, **tests** (unit and acceptance), and **documentation**. Do not invent tasks for layers the HU does not touch (for example, a read-only HU does not get a form task).
* Each task has: a **task ID** in the format `TK_DN_<sprint>_<consecutivo HU>_<n>` (example: `TK_DN_01_02_1`), a **title** that starts with an infinitive verb, a **type** (Frontend, Backend, DB, QA, Security, Documentation, DevOps), and the **acceptance criterion/criteria it covers** (by number, example: "2.3, 4").
* An HU must have between 3 and 10 tasks. If you need more than 10, state in the justification field that the HU might be too large.
* Do not add functionality that is not in the HU. If you detect a real ambiguity, do not resolve it on your own: record it in "Analyst notes" (section 6).

### **5.2 DoR — Definition of Ready (per HU)**

List of conditions that must be met **before** the team takes the HU into a sprint. It must include:

* The **general conditions** (valid for every HU): HU with role, functionality, and reason defined; acceptance criteria complete and with no pending ❓ items; success and failure messages defined; estimated by the team; prioritized; no unresolved blocking dependencies; Figma mockup or design available when the HU has a user interface.
* The **HU-specific conditions**, derived from its real dependencies (for example, "`HU_DN_01_03` finished" if the HU depends on sign-in; "user DB schema defined" if applicable; "transactional email service configured" if applicable; "availability recorded by the users" for the schedule calculation).

### **5.3 DoD — Definition of Done (per HU)**

List of conditions that must be met to consider the HU finished. It must include:

* The **general conditions**: code on the development branch with peer review; all acceptance criteria verified; unit and acceptance tests passing; validations repeated on the server; literal messages exactly matching those in the backlog; basic accessibility (do not rely on color alone, operable with the keyboard); no lint errors; documentation updated; demo approved by the Product Owner.
* The **HU-specific conditions**: security that applies (password hashing, HttpOnly cookie, role-based access control on the server, audit log), critical business rules (for example, "a direct request with the Administrator role is rejected"), and non-functional requirements cited in the HU (for example, RNF-REN-01: response ≤ 3 s at the 95th percentile).

### **5.4 Estimation — Fibonacci scale**

* Use **only** the values **1, 2, 3, 5, 8, 13, 21** (story points; it is the same sequence the SRS already uses).
* The estimate is **per HU** (not per task) and considers: technical complexity, number of fields and validation rules, number of roles and data scopes, security, external integrations (Google Calendar, email), dependencies, and risk.
* Calibration reference: **1** = minimal change with no logic (a simple action, a simple listing); **3** = form or query with moderate validations; **5** = form with several role rules and messages, or non-trivial logic; **8** = complex logic or several coordinated pieces (for example, schedule calculation); **13** = large HU with high risk. **21** = signal that the HU should be split.
* If you assign **13 or 21**, state it in "Analyst notes" and suggest how to split it (without splitting it yourself: the HU is kept as it is).
* Each estimate carries a **one-sentence justification**.
* Clarify that it is a **preliminary analyst estimate** subject to adjustment in the team's Planning Poker.

### **5.5 Priority — MoSCoW**

* Use only: **Must have, Should have, Could have, Won't have (this MVP)**.
* Decision criteria, in this order:
  1. The priority the SRS assigned to the HU and that appears in **Observation** ("SRS Priority: Critical / High / Medium / Low"). Reference: Critical → Must; High → Must or Should; Medium → Should or Could; Low → Could.
  2. **Dependencies:** an HU that others depend on (for example, sign-in, project creation, availability) rises to **Must** even if its SRS priority is lower.
  3. Value so that MVP 1 is usable end to end (authentication → project → availability → meeting).
  4. Explicit client business rule (Manager-only registration, do not create Administrators, only the creator cancels a meeting).
* **Won't** only if the HU is clearly not necessary for MVP 1; justify it.
* Each priority carries a **one-sentence justification** that cites the criterion used.
* As a quality check: in an MVP, **Must should not exceed approximately 60% of the HU**. If you exceed it, review and report.

## **6. OUTPUT FORMAT**

Deliver **one table in TSV format (tab-separated values)**, inside **a single code block**, so it can be pasted directly into the **"MVP 1"** sheet of Google Sheets. First row = headers. **No merged cells, no Markdown bullets or lists (the only bullets allowed are • inside DoR and DoD), no bold, no emojis in any column.** In particular, the Validation status field goes **without emoji** (see the following rule).

**Mandatory rule for the Validation status field:** remove any emoji and write only the text. For example, if the original says '✅ Ready', write only `Ready`. The possible values are `Ready`, `With remarks`, and `Pending clarification`. This is mandatory so that the text matches exactly the options of the drop-down menu configured in Google Sheets. Do not add leading or trailing spaces, quotes, or a final period.

**Structure: one HU = one row of type "HU" followed by its rows of type "Task"** (this allows filtering in Sheets and then importing into Jira as a story + subtasks).

**Columns, in this exact order:**

| # | Column | HU row | Task row |
| ----- | ----- | ----- | ----- |
| 1 | Type | `HU` | `Task` |
| 2 | Story Id | original ID | parent HU ID (repeated) |
| 3 | Task Id | empty | `TK_DN_xx_xx_n` |
| 4 | Epic | original | empty |
| 5 | MVP | original | empty |
| 6 | Sprint | original | empty |
| 7 | Role | original | empty |
| 8 | Feature / Functionality | original, literal (on the Task row this is the task title) | task title |
| 9 | Reason / Result | original, literal | empty |
| 10 | Acceptance Criterion | original, complete and literal, inside double quotes, one numbered item per line | criterion/criteria the task covers (e.g. 2.3, 4) |
| 11 | Validation status | original, without emoji (text only, e.g. `Ready`) | empty |
| 12 | Observation | original, literal | empty |
| 13 | Task type | empty | Frontend / Backend / DB / QA / Security / Documentation / DevOps |
| 14 | DoR | cell inside double quotes, one condition per line with a • bullet | empty |
| 15 | DoD | cell inside double quotes, one condition per line with a • bullet | empty |
| 16 | Estimate (Fibonacci SP) | number | empty |
| 17 | Estimate justification | one sentence | empty |
| 18 | MoSCoW priority | value | empty |
| 19 | Priority justification | one sentence | empty |
| 20 | Analyst notes | ambiguities, dependencies across sprints, split suggestion if SP ≥ 13; empty if there are none | empty |

Cell format rules (mandatory so that Google Sheets displays structured lists inside a single cell):

* **Acceptance Criterion, DoR, and DoD:** enclose **the entire cell content in double quotes ("")**.
* **Inside those double quotes, use real line breaks (Enter)** to separate each point. **Do not use slashes (/) or vertical bars (|) as a separator.**
* In the **Acceptance Criterion** keep the original numbering at the start of each line (1., 2., 2.1, 2.2, …), one item per line.
* In **DoR** and **DoD** start each line with a standard bullet (•), one condition per line.
* **Quotation marks of literal messages:** the messages are kept with their exact text, but inside a cell that is already enclosed in double quotes you must write them with **single quotes (' ')** instead of double or typographic quotes (“ ”), so as not to break the TSV format. Example: `Success: 'Session closed successfully.'`
* Any other cell that contains text with line breaks or with quotation marks is also enclosed in double quotes and follows the same rules; single-line cells without quotation marks do not need to be enclosed.
* If a cell must be empty, leave it empty (two consecutive tabs); do not write "N/A".
* Tabs are used only to separate columns; never inside the content of a cell.

## **7. WAY OF WORKING (so that content is neither truncated nor lost)**

The output is long. Process it in **3 deliveries, one per sprint**, and **wait for my confirmation** ("continue") between each one:

* **Delivery 1:** Sprint 1 (20 HU expected).
* **Delivery 2:** Sprint 2 (13 HU expected). **Do not repeat headers** if I ask you not to; by default do repeat them so each delivery can be pasted separately.
* **Delivery 3:** Sprint 3 (11 HU expected) + the final summary of section 8.

If a delivery does not fit in the response, **cut it at a complete HU** (never in the middle of an HU), say so, and continue when I ask.

## **8. MANDATORY FINAL VALIDATION (before closing each delivery)**

At the end of **each delivery**, outside the code block, include a brief check:

1. **Count:** HU received in the sprint vs. HU delivered. They must match. List the IDs delivered.
2. **Integrity:** confirm that elements 1 to 10 of section 4 are complete and literal in each HU. If you detect that you omitted something, correct it before delivering.
3. **Task coverage:** confirm that each criterion category (Where, Fields, Validation, Restriction, Success, Failure) of each HU has at least one associated task.
4. **Scale:** confirm that all estimates belong to {1, 2, 3, 5, 8, 13, 21} and that all priorities belong to {Must, Should, Could, Won't}.
5. **Alerts:** list the HU with SP ≥ 13, the HU with dependencies toward other sprints (for example, the notifications that depend on Sprint 9), and any ambiguity detected.

In **Delivery 3**, also add an **MVP 1 summary**: total HU, total tasks, sum of points per sprint and total, MoSCoW distribution (count and percentage of Must / Should / Could / Won't), and the final HU count against the 44 expected.

## **9. RESTRICTIONS**

* **Do not invent** rules, messages, fields, values, or requirements that are not in the backlog.
* **Do not modify** the original content of the HU or their IDs.
* **Do not use** the "Generado" tab or external knowledge to change the scope of an HU.
* **Do not split or merge** HU. If you believe an HU should be split, only suggest it in "Analyst notes".
* **Do not assign** owners or dates (the team defines that).
* If something is ambiguous, **ask or record it in "Analyst notes"** instead of assuming.

## **10. REFERENCE EXAMPLE (only to illustrate the format; do not include it in the output)**

For a simple HU such as `HU_DN_01_04 — Close my session`:

* HU row: all original fields copied literally; DoR (general conditions + "`HU_DN_01_03` finished (there is a session to close)"); DoD (general + "after closing the session, a request with the previous session is rejected"); Estimate **2**; Priority **Must have** (SRS Priority Critical; without session closing, access is not secure).
* Task rows (example): `TK_DN_01_04_1` Add the 'Close session' option to the user menu (Frontend, criterion 1) · `TK_DN_01_04_2` Implement the endpoint that invalidates the session on the server (Backend, criteria 3, 4) · `TK_DN_01_04_3` Clear the client session and redirect to sign-in, showing 'Session closed successfully.' (Frontend, criterion 5) · `TK_DN_01_04_4` Show 'The operation could not be completed. Please try again.' on an unanticipated error (Frontend, criterion 6) · `TK_DN_01_04_5` Write tests that verify rejection of requests with the previous session (QA, criterion 4).
* Example of how the Acceptance Criterion cell must look (the real content is complete; here it is abbreviated):

```
"1. Where: 'Close session' option in the user menu, visible on all authenticated views.
2. Fields: no data capture applies; the action does not require a form.
3. Validation: when the option is activated, the session is invalidated on the server and the client session is cleared.
..."
```

* Example of the DoD cell:

```
"• Code peer-reviewed and merged into the development branch.
• All acceptance criteria verified.
• After closing the session, a request with the previous session is rejected.
..."
```

---

## **11. FINAL INSTRUCTION**

Confirm in one line that you understood the rules and that you are going to start. Then count how many MVP 1 HU you received (it should be 44), report the count per sprint, and deliver **Delivery 1 (Sprint 1)**. Wait for my "continue" before moving to the next one.

**Next I paste the MVP 1 backlog ("Refinado" tab):**

[PASTE HERE ONLY SPRINTS 1, 2, AND 3 OF THE "REFINADO" TAB]
