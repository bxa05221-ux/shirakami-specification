# UI for AI API Boundary α0.1

Status: Draft / implementation boundary  
Scope: Shirakami OS external API  
Related: R0100 Shirakami Evolution Loop

## 1. Purpose

This specification defines the first provider-neutral API boundary for Shirakami OS.

The API is not an AI prompt API. It is the boundary through which a human-facing UI and external systems can exchange Landscape, Context, Protocol, Evidence, Human Decision, Execution, and Verification.

The boundary is intentionally bidirectional:

```text
AI
 ↓
Observation
 ↓
Evidence
 ↓
UI for AI
 ↓
Human
 ↓
Human Decision
 ↓
Evidence
 ↓
Protocol
 ↓
AI
```

The API therefore treats the human as an active participant in the Runtime loop, not merely as the source of an initial prompt.

## 2. Design principles

- Landscape remains the primary context.
- Evidence is a first-class boundary, not a log-only side effect.
- Human decisions are explicit and externally observable.
- Protocol execution is provider-neutral.
- AI providers are adapters, not the API itself.
- Verification compares expected and observed results.
- Mismatch remains queryable as Evidence.
- The API must not silently convert an observation into a human decision.
- The API must not authorize a human-gated transition on behalf of a human.

## 3. Minimal API surface

The canonical α0.1 surface is:

- `observe()`
- `analyze()`
- `approve()`
- `execute()`
- `verify()`
- `query_evidence()`

These operations map to the R0100 loop without exposing provider-specific implementation details.

## 4. Request/response model

### 4.1 Observe

Input:

```yaml
observation: <mapping>
context_snapshot:
  protocol_id: <string>
  landscape: <mapping>
  metadata: <mapping>
```

Output:

```yaml
state: OBSERVE | EVIDENCE
evidence: [<EvidenceRecord>]
```

### 4.2 Analyze

Input:

```yaml
protocol_id: <string>
protocol_exists: <bool>
diff_ref: <string?>
```

Output:

```yaml
protocol_id: <string>
candidate: <ProtocolCandidate?>
rationale: <string>
source_evidence: [<EvidenceRecord>]
```

A new protocol candidate enters the Human Review boundary.

### 4.3 Approve

Input:

```yaml
approved: <bool>
reviewer: <string>
```

Output:

```yaml
accepted: <bool>
state: READY | IDLE | HUMAN_REVIEW
```

Approval is an explicit human action. The API may transport the decision but must not manufacture it.

### 4.4 Execute

Input:

```yaml
protocol_id: <string>
input: <mapping>
```

The Runtime executes an already-approved protocol. The concrete AI, service, robot, database, or other external system is selected behind an Adapter boundary.

Output:

```yaml
status: <string>
transition:
  kind: <string>
  data: <mapping>
evidence: <EvidenceRecord>
```

### 4.5 Verify

Input:

```yaml
execution: <ExecutionResult>
expected_transition_kind: <string?>
diff_ref: <string?>
```

Output:

```yaml
status: pass | mismatch
uncertainty: <string>
observed: <mapping>
```

A mismatch produces immutable Mismatch Evidence containing separately addressable expected and observed values.

### 4.6 Query Evidence

Input:

```yaml
protocol_id: <string?>
signal: <string?>
transition_kind: <string?>
```

Output:

```yaml
evidence: [<EvidenceRecord>]
```

Queries are deterministic and do not mutate the Evidence Store.

## 5. Bidirectional boundary

The API must preserve the following semantic direction:

```text
External observation
      ↓
     API
      ↓
Evidence / Context
      ↓
    Human UI
      ↓
Human Decision
      ↓
     API
      ↓
Protocol / Runtime
      ↓
External system / AI
      ↓
Observation
      ↺
```

The API is therefore a boundary between two continuously interacting sides rather than a one-way command endpoint.

## 6. Transport independence

α0.1 defines semantic operations, not a mandatory transport.

An HTTP/JSON adapter, CLI, desktop UI, robot controller, or other transport may implement the same contract.

The semantic API must remain usable without a specific web framework or AI provider.

## 7. Security and authorization boundary

The API must fail closed when a required human authorization is absent.

In particular:

- `approve(approved=true)` requires an explicit human authorization marker.
- `execute()` requires the Runtime to be in an executable state.
- Invalid or unknown transitions are rejected.
- Evidence is append-only from the API perspective.
- Existing Evidence is never silently rewritten.

## 8. Relationship to R0100

R0100 defines the Evolution Loop and its Evidence semantics.

This API specification defines the external invocation boundary for that loop.

```text
R0100 Evolution Loop
        ↑
UI for AI API α0.1
        ↑
Human UI / External System
```

## 9. Non-goals

α0.1 does not define:

- an LLM provider protocol;
- a vendor-specific SDK;
- a visual UI design;
- authentication infrastructure;
- a permanent HTTP endpoint;
- complete Protocol semantics;
- automatic human decision-making.

## 10. Acceptance criteria

An implementation conforms to α0.1 when it can:

1. accept an external observation and Context Snapshot;
2. return/query resulting Evidence;
3. analyze an existing or candidate Protocol;
4. preserve an explicit Human Gate;
5. execute only an approved Protocol;
6. verify expected versus observed execution;
7. emit Mismatch Evidence when verification fails;
8. query Evidence deterministically;
9. expose the same semantics through more than one transport without changing the Runtime contract.

Core statement:

> UI for AI is the bidirectional boundary through which AI observations and human decisions become externally observable, protocol-governed, and evidence-driven Runtime state.
