# Evolution Loop × One-Stroke Route Pipeline Specification α0.3

## Status

Proposed normative integration boundary, aligned with the Runtime implementation
in `shirakami-OS`.

## Purpose

Define how a structurally generated Route Candidate enters the canonical
R0100 Evolution Loop without bypassing its Human Gate.

## Canonical flow

```text
Observation
    ↓
Evidence
    ↓
Analysis
    ↓
Protocol Candidate
    ↓
HUMAN_REVIEW
    ↓
Human Gate
    ↓
READY
    ↓
One-Stroke Runtime
    ↓
Verification
    ↓
Evidence
    ↓
ACCEPTED / DIFF
```

## Contract

### Candidate preparation

A Route Candidate:

- MUST remain a proposal until explicit human approval.
- MUST enter the existing R0100 `PROTOCOL_CANDIDATE → HUMAN_REVIEW` boundary.
- MUST preserve the ordered candidate identifiers.
- MUST NOT authorize execution.
- MUST NOT infer semantic compatibility.
- MUST NOT change Landscape state.

### Human Gate

The existing R0100 Human Gate remains the sole authorization boundary.

- Approval MUST be explicit.
- Reviewer identity MUST be retained.
- Absence of approval MUST prevent execution.
- Rejection MUST NOT execute the selected route.

The route integration MUST NOT create a second authorization state machine.

### READY and execution

After explicit approval:

- the Evolution Loop MUST enter `READY`;
- execution MUST require the `READY` state;
- One-Stroke Runtime MUST execute only the selected candidate;
- Protocol order MUST be preserved;
- the selected route MUST execute as one Runtime operation;
- missing implementations MUST fail closed.

### Verification and Evidence

After execution:

- the expected route transition MUST be verified;
- successful verification MAY advance the loop to `ACCEPTED`;
- a mismatch MUST advance the loop to `DIFF`;
- a mismatch MUST NOT silently advance to `ACCEPTED`;
- execution, human decision, and verification evidence MUST remain queryable.

## Failure behavior

The integration MUST fail closed for:

- absent human approval;
- invalid or underspecified route candidates;
- missing selected Protocol implementations;
- execution attempted outside `READY`;
- verification mismatch.

## Non-goals

This specification does not define:

- semantic compatibility inference;
- automatic route selection;
- automatic authorization;
- universal correctness claims;
- provider-specific AI behavior;
- direct modification of external reality by the Runtime.

## Reference implementation boundary

The Runtime boundary is:

`prepare_candidate() → approve_candidate() → execute() → verify() → Evidence`

The generated-candidate boundary remains:

`generate_candidates() → prepare_candidate() → Human Gate → execute() → verify()`

## Safety invariant

> A Route Candidate may enter the Evolution Loop, but only explicit human
> approval may move it from HUMAN_REVIEW to READY.

## Relationship to α0.2

α0.2 defined the Route Pipeline itself.

α0.3 defines its integration with the canonical R0100 Evolution Loop and
explicitly makes the existing Human Gate the authorization boundary.

## Evolution

Future revisions require Runtime Evidence or an explicitly reviewed stronger
contract. No semantic compatibility may be inferred merely from structural
composition.
