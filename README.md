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

## Core Specifications

- **R0100: Shirakami Evolution Loop** — externally traceable state transition and Evidence-driven evolution cycle.
  - `specifications/R0100-shirakami-evolution-loop.md`
- **UI for AI Execution Handle α0.2** — externally addressable execution session boundary.
  - `specifications/UI_FOR_AI_EXECUTION_HANDLE_ALPHA_0_2.md`
- **One-Stroke Route Pipeline α0.2** — human-gated contract from structural route candidate to verified Runtime Evidence.
  - `specifications/ONE_STROKE_ROUTE_PIPELINE_ALPHA_0_2.md`
- **Evolution Loop × One-Stroke Route Pipeline α0.3** — integration contract connecting structural route candidates to the canonical R0100 Human Gate and verified Evidence.
  - `specifications/EVOLUTION_LOOP_ONE_STROKE_ROUTE_ALPHA_0_3.md`

## What Does Not Belong Here

- Runtime implementation code
- Temporary experiments
- Unverified hypotheses
- Product-specific behavior
- Private user Landscape

## Core Principle

> Specification describes the boundary. Runtime serves the Landscape.

The specification repository exists to keep that boundary explicit and model-independent.


## License and Publication Boundary

Unless a file states otherwise, the normative specifications, protocol contracts, schemas, compatibility rules, and documentation in this repository are licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).

CC BY 4.0 permits reuse, adaptation, and commercial use with attribution. It applies only to the public normative material in this repository. It does **not** grant rights to:

- non-public native-language protocol details, prompts, operational methods, or product implementation;
- private Landscape data, credentials, deployment configuration, or service infrastructure;
- Shirakami names, logos, official certification, endorsement, or commercial branding;
- patents, trade secrets, or other rights not expressly granted by CC BY 4.0.

A document that needs protected implementation detail must link to its public normative boundary instead of reproducing that detail here. “Conforming” means meeting the stated public contract; it does not imply official certification, endorsement, or access to non-public implementations.
