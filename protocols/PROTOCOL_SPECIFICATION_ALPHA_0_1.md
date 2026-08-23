# Protocol Specification α0.1

## Status

Normative.

## Purpose

A Protocol is an executable description of an allowed interaction or transition within Shirakami OS.

The Runtime consumes Protocol definitions and executes the transitions they permit. The Runtime must not silently invent architectural meaning that is absent from the Protocol or its governing Foundation contracts.

## Matome YAML

Matome YAML is the canonical human-authored representation used to express Protocol definitions.

The current processing path is:

```text
Matome YAML
    ↓
Protocol Loader
    ↓
Protocol IR
    ↓
Runtime
    ↓
Evidence
    ↓
Landscape State
```

Protocol IR is an implementation artifact and does not replace Matome YAML as the authoring format.

## Minimum Structure

A Protocol should provide, at minimum:

- `matome.title`
- `matome.version`
- `matome.status` when applicable
- `matome.statement`
- `matome.pipeline` or equivalent executable phases when the Protocol defines a workflow

Additional fields belong to the relevant Protocol unless separately standardized.

## Loader Boundary

The Protocol Loader reads, parses, validates, and preserves the declared Protocol structure. It must not invent missing semantics.

## Runtime Boundary

The Runtime executes Protocol-defined transitions, records observable Evidence, preserves Evidence, and exposes resulting Landscape State to permitted adapters or consumers.

Protocol authority remains outside Runtime implementation.

## Evidence Boundary

Observable Runtime transitions should produce Evidence according to the active Evidence Contract. Evidence records what was observable at the transition point and must not be silently rewritten.

```text
Protocol
   ↓
Runtime Transition
   ↓
Evidence
   ↓
Landscape State
```

## Explicit Non-Goals

This specification does not define a universal cognitive ontology, fixed Landscape hierarchy, specific AI model, specific Backend, mandatory database schema, universal scheduler, or complete future Protocol language.

## Compatibility

A compatible Runtime must accept the supported Matome YAML subset, produce usable Protocol IR, execute supported transitions, preserve resulting Evidence, and avoid treating implementation-specific fields as universal semantics.

## Evolution

Extensions require evidence of a stable requirement. New theoretical concepts should return to research/design rather than being prematurely frozen here.
