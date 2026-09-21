# One-Stroke Route Pipeline Specification α0.2

## Status

Proposed normative boundary, aligned with the Runtime implementation in
`shirakami-OS`.

## Purpose

Define the contract connecting a structurally generated Protocol route
candidate to a human-gated one-stroke Runtime execution and Evidence record.

## Canonical flow

```text
Protocol Artifacts
      ↓
Structural n-gram Candidate Generation
      ↓
Human Gate
      ↓
One-Stroke Runtime Execution
      ↓
Verification
      ↓
Evidence
```

## Contract

A Route Candidate is an ordered sequence of distinct Protocol identifiers
generated from structural compatibility signals.

Candidate generation:

- MUST be conservative and structural.
- MUST NOT infer semantic compatibility.
- MUST NOT authorize execution.
- MUST NOT change Landscape state.

Human Gate:

- MUST explicitly identify the selected candidate.
- MUST identify the reviewer/approver.
- MUST reject execution when approval is absent.
- MUST remain the authorization boundary for a newly selected route.

One-Stroke Runtime:

- MUST execute only the explicitly selected candidate.
- MUST preserve Protocol order.
- MUST execute the selected route as one Runtime operation.
- MUST fail closed when a selected Protocol implementation is missing.
- MUST NOT grant authorization through composition.

Verification:

- MUST verify the expected route transition after execution.
- MUST distinguish verified execution from candidate generation.
- MUST retain the resulting Evidence.

Evidence:

- MUST preserve the selected route and execution/verification result sufficiently
  to reconstruct the observed transition.
- MUST remain an observable record rather than an authorization mechanism.

## Non-goals

This specification does not define:

- semantic compatibility inference;
- automatic route selection;
- automatic authorization;
- universal correctness of a generated route;
- provider-specific AI behavior;
- direct modification of external reality by the Runtime.

## Reference implementation boundary

The reference implementation is expected to expose an explicit boundary
equivalent to:

`generate_candidates() → select_candidate() → execute() → verify() → Evidence`

The specification defines the contract; the Runtime implements it.

## Safety invariant

> Candidate generation may suggest a route. Only an explicit Human Gate may
> authorize its execution.

## Evolution

This α0.2 contract may be revised when Runtime Evidence demonstrates a
mismatch or when a stronger, explicitly reviewed compatibility contract is
introduced. Such changes must be recorded as a new specification revision.
