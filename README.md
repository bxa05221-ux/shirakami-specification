# Shirakami Specification

## Purpose

This repository is the canonical home for **stable Shirakami specifications and protocol contracts**.

It defines what a Shirakami component **must mean and must preserve**. It does not contain the reference Runtime implementation, experiments, or product-specific applications.

## Repository Boundary

- `shirakami-model` — cognitive model, principles, and conceptual foundation
- `shirakami-research` — observations, experiments, hypotheses, and exploratory artifacts
- `shirakami-specification` — stable specifications, schemas, and protocol contracts
- `shirakami-OS` — Runtime and reference implementation, including adapters

## Direction of Evidence

Research may propose or challenge a specification. A specification becomes normative only when explicitly stabilized. The Runtime implements the specification and produces observable evidence. Evidence may return to research for further observation.

```text
Research / Observation
        ↓
   Candidate Contract
        ↓
Specification / Protocol
        ↓
Runtime / Adapter
        ↓
Evidence / Observation
        ↺
```

## What Belongs Here

- Protocol contracts
- Stable schemas
- Normative interfaces
- Compatibility rules
- Versioned specifications
- RFCs that have become normative

## What Does Not Belong Here

- Runtime implementation code
- Temporary experiments
- Unverified hypotheses
- Product-specific behavior
- Private user Landscape

## Core Principle

> Specification describes the boundary. Runtime serves the Landscape.

The specification repository exists to keep that boundary explicit and model-independent.
