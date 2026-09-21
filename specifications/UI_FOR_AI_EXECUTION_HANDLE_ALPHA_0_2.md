# UI for AI Execution Handle α0.2

Status: Alpha / External Session Contract

## Purpose

This specification extends the provider-neutral UI for AI transport boundary with an externally addressable execution session.

The execution handle is the bridge between synchronous protocol submission and a bidirectional UI-for-AI session. It gives an external UI, CLI, robot controller, or other transport a stable identifier that can be observed and verified without exposing Runtime internals.

## Execution Handle

An execution handle contains:

- `execution_id`: opaque, stable identifier for one execution
- `protocol_id`: protocol selected for the execution
- `status`: externally observable execution status
- `result`: immutable snapshot of the observable execution result

The handle is a reference, not an authority token. Possession of an execution ID does not authorize execution, approval, or mutation of Landscape.

## HTTP Contract

### POST /v1/execute

Submits a registered protocol for execution. Unknown protocols fail closed with `404`. The response includes `execution_id` together with the observable execution result.

### GET /v1/executions/{execution_id}

Returns the externally addressable execution record. Unknown IDs fail closed with `404`.

### POST /v1/executions/{execution_id}/verify

Verifies the recorded execution by handle. The request may contain `expected_transition_kind` and `diff_ref`. Verification produces a semantic `pass` or `mismatch` result. A mismatch is recorded as MISMATCH Evidence with expected and observed values kept separately. Unknown IDs fail closed with `404`.

## Immutability

The α0.2 reference implementation uses an append-only in-memory handle store. Individual execution records are immutable snapshots. Persistence is intentionally replaceable and is not part of the transport contract.

The implementation must not silently rewrite an existing execution record. Later verification produces new Evidence rather than mutating the original execution result.

## Human Authority

Execution handles do not replace the Human Gate. Explicit human authorization remains required wherever the Evolution Loop requires human review. The transport must never infer approval from an execution ID, execution status, or verification request.

## Invariants

1. `execution_id` is stable for one submitted execution.
2. Unknown execution IDs fail closed.
3. Execution records are externally inspectable and immutable.
4. Verification does not fabricate execution data.
5. Mismatch becomes queryable Evidence.
6. Expected and observed values remain separately addressable.
7. The semantic API remains usable without HTTP.
8. HTTP remains a replaceable transport, not the Runtime.
9. AI providers are not part of the contract.
10. Execution handles do not grant human authority.

## Evolution Path

Semantic API α0.1 → HTTP Transport α0.1 → Execution Handle α0.2 → Web UI / CLI / Robot UI → Adapter Contracts → External Runtime
