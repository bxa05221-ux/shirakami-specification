# Shirakami Specification Governance

## Purpose

This repository contains normative specifications and stable architectural contracts for Shirakami OS.

The repository defines **what Shirakami OS components are expected to mean and preserve**. It does not define a particular implementation, programming language, storage system, API framework, or AI model.

## Repository Boundary

- `shirakami-model` — cognitive model and conceptual foundations
- `shirakami-research` — observations, experiments, hypotheses, and exploratory artifacts
- `shirakami-specification` — normative specifications, protocol contracts, and stable architectural boundaries
- `shirakami-OS` — runtime, reference implementation, adapters, plugins, and executable artifacts

## Normative Status

A document is **Normative** when implementations claiming compatibility with Shirakami OS are expected to preserve its defined semantics or contract.

A document is **Informative** when it explains, illustrates, or contextualizes a normative specification without adding requirements.

A document is **Draft** when its semantics are still under active design and must not yet be treated as a compatibility requirement.

## Promotion Rule

Research may produce candidate designs. A candidate becomes a specification only after its boundary, semantics, and compatibility expectations are sufficiently stable.

Promotion should preserve provenance: the originating research or RFC should remain traceable.

## Implementation Rule

Specifications must remain implementation-independent unless an implementation constraint is itself part of the contract.

Runtime code, provider-specific behavior, API endpoints, storage layouts, UI details, and model-specific prompts belong in implementation repositories or adapters, not in normative specifications.

## Core Direction

```text
Research / Observation
        ↓
Candidate Design
        ↓
Specification / Contract
        ↓
Runtime / Adapter / Implementation
        ↓
Evidence / Observation
        ↺
```

The specification layer is a boundary between what has been decided and what remains implementation-dependent.
