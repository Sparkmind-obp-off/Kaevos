# KAEVOS EXECUTION CONTEXT, CREDIT EFFICIENCY & GITHUB CHECKPOINT PROTOCOL

**Document:** 49  
**Status:** ACTIVE EXECUTION PROTOCOL  
**Applies to:** KAEVOS implementation phases executed through Genspark or any equivalent coding AI  
**Repository:** `Sparkmind-obp-off/Kaevos`  
**Default branch:** `main`

---

## 1. Purpose

This document defines how KAEVOS implementation work must be packaged and executed when the coding AI has limited credits, context, or execution time.

The objective is simple:

> **Maximize verified implementation per session without allowing context size, uncontrolled scope, or missing GitHub pushes to consume the execution budget.**

This protocol exists because KAEVOS already has many architecture and implementation documents. A coding AI must **not** repeatedly ingest every historical document before every task.

The implementation process is therefore organized as:

**PHASE → SPRINT → SESSION → TASKS → TEST → VERIFY → PUSH → REMOTE CHECKPOINT**

The GitHub remote is the authoritative execution checkpoint.

---

## 2. Core Execution Rule

A unit of work is not considered complete merely because the AI:

- generated code;
- changed files;
- ran a local command;
- created a local commit;
- reported that the task is complete.

A checkpoint is complete only when:

1. the intended tasks are implemented;
2. relevant tests/validation have been run;
3. the result is reviewed against the session acceptance criteria;
4. changes are committed;
5. the commit is **pushed to the GitHub remote repository**;
6. the remote branch contains the expected commit;
7. the AI reports the remote commit SHA and changed scope.

### HARD GATE

> **NO VERIFIED GITHUB PUSH = NO COMPLETED SESSION.**

A local commit alone is **not** a valid checkpoint.

A session must not silently continue into the next session or sprint when its GitHub push has not been completed and verified.

---

## 3. Hierarchy: Phase vs Sprint vs Session vs Task

### 3.1 Phase

A **Phase** is the largest implementation boundary.

A phase represents a meaningful system capability or development layer.

Examples:

- Phase 1 — Foundation + Baseline
- Phase 2 — Data + Domain Core
- Phase 3 — API + Command Intake

A phase may contain one or more sprints.

A phase should not be fragmented unnecessarily.

---

### 3.2 Sprint

A **Sprint** is a coherent implementation package inside a phase.

A sprint should produce a meaningful increment rather than a collection of unrelated micro-edits.

A sprint may contain multiple sessions.

Example:

**Phase 3 — API + Command Intake**

- Sprint 3A — API foundation and request contracts
- Sprint 3B — command intake and validation
- Sprint 3C — command retrieval and lifecycle endpoints

The exact sprint structure must be determined from the actual repository state and phase requirements.

---

### 3.3 Session

A **Session** is one bounded AI execution window.

A session should contain several related tasks when they can be completed coherently in the same context.

A session should **not** be so small that the AI wastes most of its credits repeatedly reloading the same context.

A session should also **not** be so large that the AI spends most of its budget reading documents and never reaches implementation or GitHub push.

The target is:

> **Enough related work to produce a meaningful verified increment, but small enough to guarantee completion and remote checkpointing.**

---

### 3.4 Task

A **Task** is an atomic implementation action inside a session.

Example:

**Session 3A-01**
- implement request context middleware;
- implement API error envelope;
- add route-level validation;
- add focused tests;
- run typecheck/lint/test;
- push.

Several tasks may belong to one session when they share the same implementation context and acceptance criteria.

---

## 4. The Context Principle

The coding AI must **not** read all KAEVOS documentation by default.

The AI must use **minimum sufficient context**.

### Required context has three levels

#### Level A — Session Context

The immediate task specification.

This should normally contain:

- session objective;
- exact tasks;
- files/components likely affected;
- acceptance criteria;
- required tests;
- required GitHub checkpoint;
- explicit exclusions.

This is the primary prompt context.

#### Level B — Relevant Source-of-Truth Documents

Only the documents directly governing the current implementation should be supplied/read.

Examples:

For D1 work:
- Data Model / D1 schema;
- relevant architecture section;
- execution checklist;
- current phase prompt if needed.

For connector work:
- Connector Fabric;
- Security/Permission Model;
- Execution/Verification Model;
- relevant phase/sprint context.

For API work:
- API Contract;
- architecture;
- security;
- execution model;
- relevant phase context.

#### Level C — Repository Reality

The coding AI must inspect the actual repository files required to implement the session.

Repository code is authoritative for current implementation state.

The AI must never assume that documentation and code are already synchronized.

---

## 5. What the AI Must NOT Do

Unless explicitly required by the session, the AI must not:

- read every KAEVOS document;
- rebuild the entire architecture in its context;
- redesign completed architecture;
- rewrite unrelated code;
- implement future-phase features;
- activate reserved connectors;
- add autonomous-agent behavior;
- add unnecessary frameworks;
- introduce paid infrastructure;
- create speculative abstractions;
- spend the majority of the session producing analysis instead of implementation;
- continue into another sprint after the current checkpoint is incomplete.

---

## 6. Session Prompt Structure

Every implementation session should use this structure:

### SESSION HEADER

**Phase:**  
**Sprint:**  
**Session:**  
**Objective:**  

### READ ONLY THESE SOURCES

List only the minimum documents required.

### INSPECT THESE REPOSITORY AREAS

List exact directories/files when known.

### IMPLEMENT

List the bounded tasks.

### VALIDATE

Specify exact commands/tests expected.

### GITHUB CHECKPOINT

The AI must:

1. commit the completed work;
2. push the commit to the configured GitHub remote;
3. verify the remote branch;
4. report the resulting commit SHA.

### STOP CONDITIONS

The AI must stop if:

- required credentials are missing;
- a dependency is unavailable;
- a specification conflict is discovered;
- the implementation would require future-phase scope;
- tests reveal an unresolved blocker;
- GitHub push cannot be completed;
- verification cannot establish the required result.

### EXPLICITLY DO NOT IMPLEMENT

List nearby but excluded work.

This prevents scope expansion.

---

## 7. Recommended Session Size

A session should normally contain:

- **2–6 closely related implementation tasks**, or
- one larger coherent vertical slice.

Do not use a fixed task count mechanically.

The correct unit is the amount of work that can realistically reach:

**IMPLEMENT → TEST → VERIFY → PUSH**

inside one execution window.

If a task group cannot reasonably reach GitHub checkpointing within the available credit budget, split it before execution.

If a task group is too small and would force repeated full-context loading, combine it with the next directly related task.

---

## 8. Credit Efficiency Rules

When execution credits are limited, optimize in this order:

### 8.1 Reduce unnecessary context

Do not repeat the entire architecture.

### 8.2 Reduce speculative reasoning

The AI should inspect, decide, implement, validate.

### 8.3 Batch related tasks

Group tasks that touch the same subsystem.

### 8.4 Validate before expanding scope

Do not accumulate unverified changes.

### 8.5 Push early enough to protect work

Never wait until the entire phase is complete before creating remote checkpoints.

### 8.6 Avoid unnecessary documentation generation during coding sessions

Implementation sessions should primarily implement and validate.

Documentation changes should be included only when required for the current increment or explicitly requested.

---

## 9. Phase Completion Rule

A phase is complete only when its defined sprints have reached their required checkpoints and the phase acceptance criteria are satisfied.

Example:

**Phase 2**

Sprint 2A  
→ Session 2A-01  
→ Session 2A-02  
→ GitHub checkpoints

Sprint 2B  
→ Session 2B-01  
→ Session 2B-02  
→ GitHub checkpoints

Then:

**Phase 2 Final Validation → Phase 2 VERIFIED**

Only after that may Phase 3 begin.

---

## 10. Sprint Completion Rule

A sprint is complete only when:

- all planned sessions are completed;
- every session has a verified GitHub checkpoint;
- sprint acceptance criteria pass;
- no unresolved implementation blocker remains;
- the resulting repository state is coherent.

A sprint must not be marked complete because the AI merely says it is complete.

---

## 11. Session Completion Record

Every completed session must produce a compact report:

### Session Result

- Phase:
- Sprint:
- Session:
- Objective:
- Tasks completed:
- Files changed:
- Tests/validation:
- Result:
- Commit SHA:
- **GitHub push: VERIFIED**
- Remote branch:
- Remaining work:
- Blockers:

The report should be factual and evidence-based.

---

## 12. GitHub Push Hard Gate

The following sequence is mandatory:

```text
IMPLEMENT
   ↓
LOCAL VALIDATION
   ↓
TEST
   ↓
VERIFY
   ↓
COMMIT
   ↓
PUSH TO GITHUB
   ↓
VERIFY REMOTE COMMIT
   ↓
SESSION CHECKPOINT = COMPLETE
```

If the process stops at COMMIT:

> **SESSION = NOT COMPLETE**

If the push succeeds but the remote state is not verified:

> **SESSION = NOT COMPLETE**

If the push is verified:

> **SESSION = CHECKPOINTED**

Only a checkpointed session can unlock the next session.

---

## 13. Failure / Interruption Protocol

If the AI reaches its credit limit before pushing:

1. do not claim the session complete;
2. do not start the next session;
3. preserve the current state if possible;
4. inspect the repository state;
5. determine exactly what remains;
6. resume with a narrow recovery session;
7. push and verify the recovery result;
8. only then continue.

The recovery session should **not** reread the entire architecture.

It should receive:

- current task state;
- files changed;
- remaining acceptance criteria;
- relevant source-of-truth documents only;
- GitHub checkpoint requirement.

---

## 14. Context Recovery Protocol

When a new AI session starts after an interruption, the AI should first inspect:

1. current branch;
2. latest remote commit;
3. current working tree;
4. recent relevant files;
5. session checkpoint/report.

Then continue from the last verified checkpoint.

The AI must not assume that unpushed local work exists unless the execution environment actually preserves it.

---

## 15. Documentation Authority Model

The KAEVOS documentation set is layered.

The AI should treat documents as **target specifications**, while the current repository is the implementation reality.

When documents conflict:

1. identify the conflict;
2. do not silently choose;
3. stop the affected implementation;
4. report the conflict;
5. resolve the specification before continuing.

For routine implementation, only the relevant documents should be loaded.

---

## 16. Phase / Sprint / Session Planning Template

Use this structure for every future phase:

```text
PHASE N
│
├── Sprint N-A
│   ├── Session N-A-01
│   │   ├── Task 1
│   │   ├── Task 2
│   │   ├── Task 3
│   │   └── TEST → VERIFY → PUSH
│   │
│   └── Session N-A-02
│       ├── Task 4
│       ├── Task 5
│       └── TEST → VERIFY → PUSH
│
├── Sprint N-B
│   ├── Session N-B-01
│   └── Session N-B-02
│
└── PHASE VALIDATION
    ├── Sprint acceptance
    ├── Integration validation
    └── PHASE VERIFIED
```

---

## 17. Example: Phase 3

Phase 3 should not be sent to Genspark as one giant implementation prompt.

Instead:

### Phase 3 — API + Command Intake

**Sprint 3A — API Foundation**

Possible sessions:

- Session 3A-01: API routing and request context
- Session 3A-02: validation and response envelopes
- Session 3A-03: health/connectivity/API tests

Each session:

**implement → test → verify → push**

Then Sprint 3A is reviewed.

Only after Sprint 3A is checkpointed should Sprint 3B begin.

### Sprint 3B — Command Intake

Possible sessions:

- Session 3B-01: command creation contract
- Session 3B-02: command persistence
- Session 3B-03: command retrieval
- Session 3B-04: lifecycle/error handling

Again:

**each session must reach a verified GitHub checkpoint.**

The exact breakdown must be created from the actual Phase 3 implementation requirements and repository state rather than invented prematurely.

---

## 18. Genspark Master Prompt Rule

A master phase prompt may define the overall phase, but it must not require the AI to implement the entire phase in one uncontrolled execution.

The preferred pattern is:

**Phase Master Prompt**
→ defines phase boundaries

**Sprint Prompt**
→ defines one coherent implementation package

**Session Prompt**
→ defines the exact execution window

This prevents the AI from treating a phase-level document as permission to implement everything at once.

---

## 19. Human Control Rule

The human operator remains the final authority over:

- moving to the next sprint;
- moving to the next phase;
- accepting a checkpoint;
- approving consequential external actions;
- resolving specification conflicts;
- activating production integrations.

The AI may recommend the next step but must not silently expand scope.

---

## 20. Definition of Done for a Session

A session is **DONE** only if all are true:

- [ ] Correct phase identified
- [ ] Correct sprint identified
- [ ] Session scope is bounded
- [ ] Relevant context only was loaded
- [ ] Required implementation completed
- [ ] Tests/validation executed
- [ ] No known critical regression
- [ ] Changes committed
- [ ] Commit pushed to GitHub
- [ ] Remote branch verified
- [ ] Commit SHA recorded
- [ ] Session report produced
- [ ] No unauthorized scope expansion

If any mandatory item is false:

> **SESSION = NOT DONE**

---

## 21. Definition of Done for a Sprint

- [ ] All planned sessions checkpointed
- [ ] All required session acceptance criteria pass
- [ ] Remote GitHub state is current
- [ ] Sprint integration checks pass
- [ ] No unresolved blocker
- [ ] Sprint report recorded
- [ ] Human/operator approves progression

---

## 22. Definition of Done for a Phase

- [ ] All required sprints complete
- [ ] All session checkpoints verified
- [ ] Phase-level tests pass
- [ ] Integration behavior validated
- [ ] Security constraints remain intact
- [ ] No false capability claims
- [ ] GitHub remote contains the complete phase result
- [ ] Phase acceptance criteria pass
- [ ] Phase marked VERIFIED
- [ ] Next phase explicitly authorized

---

## 23. Non-Negotiable Rules

1. **Do not read everything by default.**
2. **Use minimum sufficient context.**
3. **Group related tasks into meaningful sessions.**
4. **Do not make sessions artificially tiny.**
5. **Do not make sessions so large that they cannot reach a checkpoint.**
6. **Test before claiming completion.**
7. **Commit is not the checkpoint.**
8. **GitHub PUSH + remote verification is the checkpoint.**
9. **No verified push = no completed session.**
10. **No completed session = no next session.**
11. **No completed sprint = no next sprint.**
12. **No verified phase = no next phase.**
13. **Never silently expand scope.**
14. **Never claim VERIFIED without evidence.**
15. **Protect the credit budget by minimizing repeated context loading.**

---

## 24. Final Operating Principle

KAEVOS implementation should not be:

**READ EVERYTHING → THINK EVERYTHING → BUILD EVERYTHING → RUN OUT OF CREDITS**

It should be:

**RELEVANT CONTEXT → BOUNDED TASK BATCH → IMPLEMENT → TEST → VERIFY → PUSH → REMOTE CHECKPOINT → NEXT**

The goal is not to minimize the amount of work per session.

The goal is to maximize the amount of **verified, remotely preserved work per session**.

> **Context should be narrow. Work should be meaningful. Verification should be real. GitHub push is the checkpoint. Progression is gated by evidence.**

