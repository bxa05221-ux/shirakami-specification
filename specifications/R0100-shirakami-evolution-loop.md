# R0100: Shirakami Evolution Loop

**Status:** Normative Draft  
**Version:** 1.0  
**Scope:** Shirakami Specification  
**Role:** Core Evolution Loop / Evidence-driven state transition contract

---

## 1. Purpose

Shirakami Evolution Loop defines the externally traceable cycle by which observations become Evidence, Evidence informs Analysis, Analysis may produce a Protocol Candidate, human review authorizes adoption, Runtime executes the approved Protocol, Verification evaluates the result, and the resulting transition becomes Evidence for the next cycle.

```text
Observation
→ Evidence
→ Analysis
→ Protocol
→ Runtime
→ Verification
→ Evidence
↺
```

AI may assist observation, analysis, simulation, proposal, and verification. AI does not acquire final authority over Protocol adoption or human-required transitions.

---

## 2. Core Transition Model

```text
δ(S, E, G) → (A, S')
```

Where:

- `S` = current State
- `E` = Event
- `G` = Guard
- `A` = Action
- `S'` = next State

Every normal, failure, and safety transition uses the same basic contract.

```yaml
transition:
  id: Txxx
  from: STATE
  event: EVENT
  guard: CONDITION
  action:
    - ACTION
  to: STATE
```

---

## 3. States

```yaml
states:
  IDLE:
    type: waiting
  OBSERVE:
    type: observation
  EVIDENCE:
    type: evidence_creation
  ANALYZE:
    type: analysis
  PROTOCOL_CANDIDATE:
    type: proposal
  HUMAN_REVIEW:
    type: human_gate
  READY:
    type: execution_ready
  EXECUTE:
    type: execution
  VERIFY:
    type: verification
  ACCEPTED:
    type: accepted
  DIFF:
    type: discrepancy_analysis
  STOPPED:
    type: safety_stop
    terminal: true
```

---

## 4. Normal Lifecycle

```text
IDLE
 ↓
OBSERVE
 ↓
EVIDENCE
 ↓
ANALYZE
 ├── existing protocol → READY
 └── new protocol → PROTOCOL_CANDIDATE
                         ↓
                   HUMAN_REVIEW
                    ├─ approve → READY
                    ├─ revise  → PROTOCOL_CANDIDATE
                    └─ reject  → IDLE
                         ↓
                      EXECUTE
                         ↓
                      VERIFY
                    ┌────┴────┐
                    ↓         ↓
                ACCEPTED    DIFF
                    ↓         ↓
                 OBSERVE   ANALYZE
```

---

## 5. Failure Lifecycle

Failure does not automatically mean STOP.

```text
Failure
 ↓
Classify
 ├─ RETRY           → current state
 ├─ REOBSERVE       → OBSERVE
 ├─ REVISE          → PROTOCOL_CANDIDATE
 ├─ HUMAN_DECISION  → HUMAN_REVIEW
 └─ STOP            → STOPPED
```

Failure classes:

- `INVALID`
- `INCOMPLETE`
- `MISMATCH`
- `UNRESOLVED`
- `PERSISTENCE_FAILURE`
- `SAFETY_STOP`

Unknown states and undefined transitions fail closed.

---

## 6. Human Authority

The following transitions require explicit human authorization:

- Protocol adoption
- Protocol revision adoption
- Any explicitly human-gated transition
- Real-world side effects designated as human-controlled

The following are prohibited:

- AI-only Protocol adoption
- Implicit approval
- Timeout interpreted as approval
- Inconclusive verification interpreted as acceptance

```yaml
human_gate:
  protocol_adoption:
    required: true
  ai_only_approval:
    allowed: false
  timeout_as_approval:
    allowed: false
```

---

## 7. Evidence

Evidence is externally represented, traceable, and immutable.

### 7.1 Evidence Types

```yaml
evidence_types:
  - OBSERVATION
  - FAILURE
  - MISMATCH
  - PROTOCOL
  - SAFETY
  - HUMAN_DECISION
  - CONTEXT_SNAPSHOT
```

`CONTEXT_SNAPSHOT` records the relevant external execution context needed to reconstruct the state in which an observation, execution, or verification occurred.

### 7.2 Evidence Schema

```yaml
evidence:
  id: EV-xxxxx
  type: OBSERVATION

  source:
    type: SOURCE_TYPE
    id: SOURCE_ID

  observation:
    statement: TEXT

  context:
    snapshot_ref: CTX-xxxxx
    loop_id: LOOP-xxxxx
    protocol_id: PROTO-xxxxx
    execution_id: EXE-xxxxx
    verification_id: VER-xxxxx

  lineage:
    caused_by: []
    generated_by: []
    affects: []
    verified_by: []

  provenance:
    created_at: TIMESTAMP
    created_by: ACTOR
    source_type: SOURCE_TYPE

  integrity:
    immutable: true
    hash: HASH
```

Evidence is not silently overwritten or deleted. Corrections are represented by new Evidence linked through lineage.

---

## 8. Transition Record

Every meaningful transition attempt is externally recorded.

```yaml
transition_record:
  id: TR-xxxxx
  from_state: STATE
  event: EVENT
  guard_result: true
  action:
    - ACTION
  to_state: STATE
  result: allowed
  timestamp: TIMESTAMP
  evidence_refs:
    - EV-xxxxx
```

Rejected transitions are also recorded.

Routine transitions may remain Runtime logs when they contain no information relevant to future reasoning.

---

## 9. Transition → Evidence

A Transition Record is promoted to Evidence when it has information value for subsequent reasoning, verification, protocol revision, human review, or safety handling.

```yaml
evidence_promotion:
  promote_when:
    - transition_result == "rejected"
    - transition_failure == true
    - mismatch_detected == true
    - protocol_change_signal == true
    - safety_condition_detected == true
    - human_review_requested == true
    - execution_result_changes_future_reasoning == true
```

Thus:

```text
State
 ↓
Transition
 ↓
Transition Record
 ↓
Evidence
 ↓
Analysis
```

---

## 10. Evidence Store

The Evidence Store provides:

```yaml
evidence_store:
  create:
    input: Evidence
    output: EvidenceID

  get:
    input: EvidenceID
    output: Evidence

  query:
    input: EvidenceQuery
    output: EvidenceSet

  append:
    input: Evidence
    output: EvidenceID

  verify:
    input: EvidenceID
    output: IntegrityResult
```

The minimum query dimensions are:

- Evidence type
- Protocol reference
- Execution reference
- Source
- Lineage
- Time range
- Active status

---

## 11. Context Snapshot

Execution Context is externally represented rather than treated as hidden Runtime memory.

```yaml
context_snapshot:
  id: CTX-xxxxx

  loop_id: LOOP-xxxxx

  state:
    current: STATE

  protocol:
    id: PROTO-xxxxx
    version: VERSION

  execution:
    id: EXE-xxxxx

  inputs:
    ...

  environment:
    ...

  created_at: TIMESTAMP

  integrity:
    immutable: true
    hash: HASH
```

A Context Snapshot may be referenced by Evidence but does not replace Evidence.

---

## 12. Analysis

Analysis consumes:

```text
Current Observation
+
Relevant Evidence
+
Current Protocol
+
Context Snapshot
```

The result is a proposal for the next transition, not final authority.

```yaml
analysis_result:
  id: AN-xxxxx
  status:
    - complete
    - insufficient
    - unresolved

  protocol_match: true | false

  protocol_change_required: true | false

  evidence_refs:
    - EV-xxxxx

  uncertainty:
    level: ...

  recommendation:
    type:
      - EXISTING_PROTOCOL
      - PROTOCOL_CANDIDATE
      - OBSERVE_MORE
      - HUMAN_REVIEW
```

---

## 13. Protocol Candidate

A Protocol Candidate must be traceable to Evidence and, when generated from discrepancy analysis, to a Diff.

```yaml
protocol_candidate:
  id: PC-xxxxx

  based_on:
    evidence:
      - EV-xxxxx
    analysis:
      - AN-xxxxx

  diff_ref: DIFF-xxxxx

  change:
    target: PROTO-xxxxx
    reason: TEXT
    proposed_change: TEXT

  status: candidate

  requires_human_approval: true
```

No Protocol Candidate becomes an active Protocol without the required Human Review.

---

## 14. Diff

Diff identifies a discrepancy between expected and observed behavior.

```yaml
diff:
  id: DIFF-xxxxx

  expected: ...

  observed: ...

  evidence_refs:
    - EV-xxxxx

  protocol_change_required: true | false

  additional_observation_required: true | false

  status:
    - complete
    - incomplete
    - unresolved
```

Diff is not itself a Protocol change. It is an analysis object that may produce a Protocol Candidate.

---

## 15. Execution

Execution requires an approved Protocol and a valid Context Snapshot.

```yaml
execution:
  id: EXE-xxxxx

  protocol_id: PROTO-xxxxx

  approved_by:
    type: human
    review_id: REVIEW-xxxxx

  context_snapshot:
    id: CTX-xxxxx

  status:
    - ready
    - running
    - completed
    - failed
    - partial
    - timeout
```

---

## 16. Verification

Verification compares expected and observed results.

```yaml
verification:
  id: VER-xxxxx

  execution_id: EXE-xxxxx

  expected: ...

  observed: ...

  status:
    - pass
    - mismatch
    - inconclusive

  uncertainty:
    level: ...

  evidence_refs:
    - EV-xxxxx
```

Verification uncertainty is explicitly represented.

`uncertainty` is preferred to a single confidence value because Shirakami requires unresolved uncertainty to remain externally visible rather than being compressed into apparent certainty.

---

## 17. Runtime Contract

```yaml
shirakami_runtime_contract:

  state:
    externalized: true
    traceable: true

  transition:
    validated: true
    recorded: true

  failure:
    classified: true
    safety: fail_closed

  evidence:
    immutable: true
    traceable: true
    queryable: true
    lineage_preserved: true

  context:
    externally_represented: true
    snapshotable: true

  protocol:
    evidence_required: true
    human_approval_required: true

  execution:
    approved_protocol_required: true
    context_snapshot_required: true

  verification:
    required: true
    uncertainty_externalized: true

  evolution:
    result_to_evidence: true
    evidence_to_analysis: true
    analysis_to_protocol_candidate: true
    diff_to_protocol_candidate: true

  authority:
    final_human_authority: true
```

---

## 18. Invariants

```yaml
invariants:

  - Every PROTOCOL_CANDIDATE references Evidence.

  - A PROTOCOL_CANDIDATE cannot become READY
    without required HUMAN_REVIEW approval.

  - Every EXECUTE transition references an approved Protocol.

  - Every execution references a Context Snapshot.

  - Every meaningful transition is traceable.

  - Every verification mismatch produces Evidence.

  - Every STOPPED state contains a stop reason.

  - AI output alone cannot authorize a human-required transition.

  - Existing Evidence is never silently overwritten.

  - Recoverable failures have explicit recovery paths.

  - Unresolved or inconclusive results never automatically become ACCEPTED.

  - Timeout never means approval.

  - Unpersisted state is not treated as completed.

  - Unknown states and undefined transitions fail closed.

  - Original failure Evidence remains traceable after recovery.

  - Evidence lineage remains intact across Loop iterations.

  - Context Snapshots used for reasoning or execution remain externally traceable.
```

---

## 19. Self-Verification

The Evolution Loop verifies its own Runtime behavior.

Required checks:

- Transition validity
- Guard validity
- Evidence integrity
- Evidence lineage
- Human Gate enforcement
- State consistency
- Failure recovery
- Fail-closed behavior
- Context Snapshot integrity
- Verification uncertainty handling
- Loop closure

Self-verification does not grant the Runtime authority to self-adopt a Protocol.

---

## 20. Compact Form

```text
OBSERVE
→ EXTERNALIZE
→ EVIDENCE
→ QUERY
→ ANALYZE
→ PROTOCOLIZE
→ HUMAN REVIEW
→ EXECUTE
→ VERIFY
→ RECORD
→ REUSE
→ OBSERVE
```

Failure:

```text
FAILURE
→ CLASSIFY
→ RETRY / REOBSERVE / REVISE / HUMAN_DECISION / STOP
```

---

## 21. Normative Principle

> **Shirakami evolution is not AI self-modification.**

> It is the externally traceable process by which observations become Evidence, Evidence informs Protocol, human authority governs adoption, Runtime executes the approved structure, Verification tests the result, and the result becomes Evidence for the next iteration.

The purpose of the Evolution Loop is therefore not to make AI autonomous.

It is to make **human–AI interaction cumulative, traceable, verifiable, and recoverable without surrendering human final authority.**

---

## 22. Implementation Boundary

R0100 defines the specification boundary.

Reference implementation belongs in `shirakami-OS`.

The expected implementation chain is:

```text
R0100 Specification
        ↓
Transition Validator
        ↓
State Machine
        ↓
Transition Record
        ↓
Evidence Store
        ↓
Evidence Query
        ↓
Analysis
        ↓
Protocol Candidate
        ↓
Human Gate
        ↓
Execution
        ↓
Verification
        ↓
Evidence
        ↺
```

Implementation failures and missing capabilities are themselves Evidence and may result in a future revision of R0100.

---

## 23. Status

R0100 is a normative draft intended for integration with the Shirakami Runtime and subsequent verification through implementation.

**One change, one verification.**
