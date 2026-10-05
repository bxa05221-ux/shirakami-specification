# Shirakami API α0.1 — Human Authority API Boundary

**Status:** Proposal / Review Required  
**Version:** α0.1  
**Scope:** Shirakami Specification  
**Role:** Provider-neutral API contract for the Architecture v0.2 authority boundaries

## 1. Purpose

Shirakami API α0.1 defines a transport-neutral API boundary for exposing the Shirakami Architecture v0.2 state-transition model to external clients.

The API is not an AI API and does not grant a Model, Runtime, Adapter, Projection, Renderer, or API client authority over canonical Landscape state.

Canonical flow:

```text
Reality → Observation → Model Analysis → Proposal → Human Gate → Landscape → Projection → Renderer → User
```

The API MUST preserve the distinction between observation and interpretation; proposal and accepted state; Human Gate decision and model output; canonical Landscape and Projection; Projection and Renderer; expected and observed results; Evidence and subsequent interpretation.

## 2. Design Principle

> **The API exposes authority boundaries; it does not move authority into the API.**

A successful HTTP response, model response, renderer response, execution handle, or verification result MUST NOT by itself constitute authorization to modify canonical Landscape state.

Human authority remains explicit.

## 3. Transport Independence

The semantic API contract is independent of HTTP. HTTP is the first reference transport. Implementations MAY expose the same contract through HTTP/REST, RPC, CLI, message transport, or embedded library calls. Transport-specific behavior MUST NOT change authority semantics.

## 4. Core Resources

| Resource | Authority role |
|---|---|
| Observation | externally recorded observation |
| Evidence | traceable supporting record |
| Proposal | non-authoritative candidate state |
| Human Gate Decision | authorization decision |
| Landscape | canonical accepted state |
| Projection | read-side view of Landscape |
| Render Result | presentation output |
| Provenance | transition lineage |
| Mismatch | expected-vs-observed discrepancy |

## 5. State Boundaries

### 5.1 Observation

```yaml
observation:
  id: OBS-xxxxx
  statement: TEXT
  evidence_refs: []
  source:
    runtime: ...
    model: ...
  created_at: TIMESTAMP
```

Observation MUST NOT silently become accepted Landscape state.

### 5.2 Proposal

```yaml
proposal:
  id: PROP-xxxxx
  observation_refs: []
  evidence_refs: []
  proposed_changes: []
  source: {}
  status: pending | accept | reject | revise
  provenance: {}
```

A Proposal is never canonical merely because it was generated.

### 5.3 Human Gate

```yaml
human_gate:
  proposal_id: PROP-xxxxx
  decision: accept | reject | revise
  decided_by: HUMAN_ID
  timestamp: TIMESTAMP
  reason: TEXT
```

Only explicit `accept` MAY advance accepted Landscape state.

The API MUST NOT interpret HTTP 2xx, timeout, model confidence, absence of rejection, verification pass, execution completion, or client disconnect as acceptance.

### 5.4 Landscape

```yaml
landscape:
  id: LAND-xxxxx
  version: VERSION
  context: {}
  observations: []
  evidence: []
  accepted_state: []
  provenance: []
  snapshots: []
```

Canonical Landscape state MUST be changed only through the authorized Human Gate acceptance path.

### 5.5 Projection

```yaml
projection:
  id: PROJ-xxxxx
  landscape_id: LAND-xxxxx
  landscape_version: VERSION
  view: VIEW
  observation_axis: []
  parameters: {}
```

Projection is read-side and is not a second source of truth. Projection parameters MUST NOT mutate Landscape.

### 5.6 Render Result

```yaml
render_result:
  id: RENDER-xxxxx
  projection_id: PROJ-xxxxx
  landscape_version: VERSION
  output: {}
  renderer: {}
```

Renderer MUST NOT mutate canonical Landscape through rendering.

## 6. Semantic Operations

```text
create_observation(observation) → observation_id
create_proposal(proposal) → proposal_id
decide_human_gate(proposal_id, decision) → decision_record
get_landscape(landscape_id, version?) → landscape_snapshot
project(landscape_id, projection_spec) → projection
render(projection_id, renderer_spec) → render_result
create_evidence(evidence) → evidence_id
query_evidence(query) → evidence_set
```

For `accept`, Human Gate MUST create a new Landscape version. For `reject` and `revise`, accepted Landscape MUST remain unchanged.

## 7. Reference HTTP Mapping

```text
POST /v1/observations
GET  /v1/observations/{observation_id}
POST /v1/proposals
GET  /v1/proposals/{proposal_id}
POST /v1/proposals/{proposal_id}/human-gate
GET  /v1/landscapes/{landscape_id}
GET  /v1/landscapes/{landscape_id}/versions/{version}
POST /v1/projections
GET  /v1/projections/{projection_id}
POST /v1/render
GET  /v1/render/{render_id}
POST /v1/evidence
GET  /v1/evidence/{evidence_id}
GET  /v1/evidence
POST /v1/mismatches
GET  /v1/mismatches/{mismatch_id}
```

There is deliberately no generic endpoint for arbitrary canonical Landscape mutation. Canonical mutation occurs through the authorized Human Gate transition.

## 8. Error Semantics

| Error | Meaning |
|---|---|
| `NOT_FOUND` | referenced resource does not exist |
| `INVALID_REQUEST` | request violates schema |
| `INVALID_TRANSITION` | transition is not permitted |
| `HUMAN_GATE_REQUIRED` | explicit authorization is required |
| `PROPOSAL_NOT_PENDING` | Proposal cannot receive another decision |
| `VERSION_CONFLICT` | Landscape version is stale |
| `PROVENANCE_INCOMPLETE` | required lineage is missing |
| `MISMATCH` | expected and observed states differ |
| `INCONCLUSIVE` | result is insufficient for acceptance |
| `SAFETY_STOP` | operation is stopped by safety boundary |

HTTP status codes MAY map these errors, but MUST NOT alter their semantic meaning.

## 9. Concurrency and Versioning

Landscape versions are monotonic:

```text
Landscape vN → Proposal → Human Gate: accept → Landscape vN+1
```

Rejected and revised proposals MUST NOT create accepted-state versions.

Clients SHOULD provide the Landscape version they observed. Implementations SHOULD reject stale writes with `VERSION_CONFLICT` rather than silently overwriting newer accepted state.

## 10. Provenance Contract

Minimum accepted-state lineage:

```text
Landscape vN+1
 ← Human Gate Decision
 ← Proposal
 ← Observation / Evidence
 ← source Model / Runtime / Adapter
 ← handoff / execution context where applicable
```

A conforming implementation MUST be able to answer: what changed, which Proposal caused it, who authorized it, which Observation/Evidence supported it, which Model/Runtime/Adapter produced the source material, and which Landscape version preceded the change.

## 11. Idempotency

State-changing operations SHOULD support an idempotency key. Repeated submission of the same Human Gate decision MUST NOT create duplicate accepted Landscape versions.

Idempotency prevents duplicate execution; it does not bypass Human Gate.

## 12. Authentication and Authorization

Authentication is implementation-specific. Authorization is architectural.

A valid authenticated client is not automatically authorized to change canonical Landscape. The API MUST distinguish identity, authentication, authorization, and Human Gate authority.

A Model, Runtime, Adapter, Renderer, or API token MUST NOT be treated as a human authority merely because it is authenticated.

## 13. Model and Runtime Independence

The API MUST NOT require a particular AI model, provider, Runtime, programming language, database, renderer, or UI framework.

Replacing a Model or Runtime MUST NOT change the authority boundary. Source metadata SHOULD identify the actual components involved so provenance survives substitution.

## 14. Conformance Requirements

Implementations claiming α0.1 conformance SHOULD verify:

- API-001 Proposal Separation
- API-002 Human Gate Acceptance
- API-003 Reject Preservation
- API-004 Revise Preservation
- API-005 Projection Read-Only
- API-006 Renderer Read-Only
- API-007 Provenance
- API-008 Monotonic Versioning
- API-009 Model/Runtime/Renderer Substitution
- API-010 Mismatch Preservation
- API-011 Fail Closed
- API-012 Idempotency

## 15. Reference Round Trips

Normal:

```text
Reality → Observation → Evidence → Model Analysis → Proposal → Human Gate → Landscape vN+1 → Projection → Renderer → User
```

Reject:

```text
Proposal → Human Gate: reject → Evidence / provenance → Landscape unchanged
```

Mismatch:

```text
Execution / Observation → Verification → Expected ≠ Observed → Mismatch Evidence → Analysis → Proposal → Human Gate
```

## 16. Security and Safety Boundary

The API MUST assume generated content can be incorrect.

It MUST NOT infer authority from model confidence, successful parsing, successful execution, successful rendering, verification pass, repeated agreement, or model consensus.

Consensus MAY be recorded as Evidence or Analysis input, but it does not become canonical truth without the required Human Gate.

Unknown states, invalid transitions, missing required provenance, and stale canonical versions SHOULD fail closed.

## 17. Relationship to Existing Specifications

This specification extends, rather than replaces:

- R0100 Shirakami Evolution Loop — state transition and Human Gate;
- UI for AI Execution Handle α0.2 — externally addressable execution boundary;
- One-Stroke Route Pipeline — human-gated execution and verification;
- Shirakami Architecture v0.2 — authority boundaries for Proposal, Human Gate, Landscape, Projection, Renderer, Provenance, substitution, and mismatch.

The API is an access contract over these boundaries, not a new Runtime architecture.

## 18. Explicit Non-Goals

This specification does not define a new LLM, autonomous-agent authority model, mandatory database, mandatory authentication provider, mandatory cloud service, ThreadRPG-specific API, UI framework, product-specific business API, private Landscape storage, or implementation-specific Python classes.

## 19. Status and Promotion Path

This document is a proposal and is not yet a stable normative API.

Promotion requires schema and terminology review, reference semantic implementation, conformance tests, transport-level verification, compatibility review with existing specifications, explicit Human Gate approval, and repository merge.

**One change, one verification.**