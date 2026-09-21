# UI for AI HTTP Transport Boundary α0.1

Status: Alpha / Transport Contract

## Purpose

This specification defines the first HTTP transport boundary for the
provider-neutral **UI for AI API**.

The transport is not the Runtime. It translates HTTP/JSON requests into the
semantic API and returns observable results without making a model provider
part of the contract.

## Boundary

```text
Human / UI / External System
            ↓
        HTTP / JSON
            ↓
     UI for AI API
            ↓
 Evidence-driven Runtime
            ↓
 Adapter / Backend / AI
            ↓
       Observation
            ↺
```

The boundary is bidirectional. Human decisions can return to the Runtime as
explicitly authorized actions; authorization must never be inferred from an
HTTP request merely because it contains an approval field.

## Endpoints

### GET /health

Liveness endpoint.

Response:

```json
{"status":"ok"}
```

### POST /v1/observe

Accepts an observation and an explicit Context Snapshot.

```json
{
  "observation": {},
  "context": {
    "protocol_id": "example",
    "landscape": {},
    "metadata": {}
  }
}
```

The Runtime records the Context Snapshot as Evidence.

### POST /v1/analyze

Requests analysis against a protocol.

```json
{
  "protocol_id": "example",
  "protocol_exists": true,
  "diff_ref": ""
}
```

### POST /v1/approve

Carries a Human Gate decision.

```json
{
  "approved": true,
  "reviewer": "human",
  "human_authorized": true
}
```

`human_authorized` is an explicit authorization signal. A missing or false
value must not be interpreted as approval.

### POST /v1/execute

Executes a protocol registered by the host transport.

```json
{
  "protocol_id": "example",
  "input_data": {}
}
```

The transport must not dynamically resolve arbitrary code or provider names
from request data. Protocol registration remains an application-side boundary.

### GET /v1/evidence

Queries the Evidence boundary.

Supported filters:

- `protocol_id`
- `signal`
- `transition_kind`

No filter returns the current queryable Evidence set.

## Verification Boundary

Verification remains a semantic Runtime operation in α0.1.

HTTP execution currently returns observable execution data, but does not invent
a persistent execution handle. A future transport revision may introduce an
execution identifier and a corresponding verification endpoint.

Until that contract exists, the HTTP layer must not fabricate verification
state.

## Invariants

1. HTTP is a transport concern, not a Runtime concern.
2. The semantic API remains usable without HTTP.
3. AI providers are not named in the transport contract.
4. Human authorization is explicit.
5. Evidence remains externally queryable.
6. Context Snapshot remains explicit.
7. Unknown protocol execution fails closed.
8. The transport must not silently convert an observation into a human decision.
9. Verification data must not be fabricated.
10. The transport may be replaced by CLI, desktop, mobile, robot, or another
   interface without changing the semantic Runtime contract.

## Evolution Path

```text
Semantic API α0.1
      ↓
HTTP Transport α0.1
      ↓
Execution Handle α0.2
      ↓
Web UI / CLI / Robot UI
      ↓
Adapter Contracts
      ↓
External Runtime
```

The purpose of this boundary is not to make HTTP the permanent interface.
It is to demonstrate that the same bidirectional UI-for-AI semantics can cross
an external transport boundary without moving authority into the AI backend.
