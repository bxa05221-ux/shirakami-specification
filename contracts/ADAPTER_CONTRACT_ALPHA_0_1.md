# Adapter Contract α0.1

## Status

Normative.

## Purpose

An Adapter connects Shirakami OS Runtime to an external Backend while preventing Backend-specific behavior from becoming part of Runtime Core.

```text
Landscape
   ↓
Protocol
   ↓
Runtime
   ↓
Adapter
   ↓
Backend
```

## Responsibilities

An Adapter translates between Runtime-level operations and Backend-specific operations. It may provide:

- read access required by an active operation;
- controlled writes explicitly permitted by Runtime and Protocol;
- translation of Backend events/results into Runtime-observable forms;
- preservation of Backend provenance needed for Evidence.

## Runtime Boundary

Runtime Core must not require GitHub-, database-, filesystem-, or other Backend-specific APIs in order to execute a Protocol.

Runtime remains responsible for Protocol execution, transition control, Evidence recording, Landscape State exposure, and Runtime-level write boundaries.

## Controlled Write

A write is permitted only when:

1. Runtime has an operation that permits it;
2. the active Protocol permits or requires the transition;
3. the Adapter can represent the operation safely in the target Backend;
4. sufficient Evidence/provenance can be preserved.

An Adapter must not independently invent transitions.

## Observation / Read-back

When a controlled write changes Backend state, the Adapter should support read-back or equivalent observation so the resulting state can be compared with the intended transition.

```text
Runtime Intent
     ↓
Controlled Write
     ↓
Backend
     ↓
Read-back / Observation
     ↓
Evidence
```

## Backend Independence

Adapters need not expose identical capabilities. Each Adapter must expose only capabilities it can safely support, and Runtime must not assume a capability exists merely because another Adapter provides it.

## GitHub Adapter

GitHub is the current concrete Backend used by Shirakami OS. Its Adapter demonstrates this boundary; GitHub is not a required dependency of Runtime Core.

## Provenance

Backend-originated observations affecting Landscape State should retain enough provenance to identify Backend context and relevant operation. Adapters translate Backend facts; they do not rewrite them into architectural conclusions.

## Out of Scope

This α0.1 contract does not define universal plugin lifecycle, authentication implementation, authorization policy language, conflict-resolution algorithms, arbitrary multi-Backend synchronization, mandatory capability schema, or automatic multi-Backend orchestration.

## Compatibility

An α0.1-compatible Adapter must remain behind the Runtime/Adapter boundary, expose only safe operations, honor controlled-write constraints, support observation/read-back where required, preserve relevant provenance, and avoid embedding Backend-specific assumptions into Runtime Core.

## Evolution

The contract should grow only when implementation or observation demonstrates a stable cross-Backend requirement.
