# TSUGARU GUIDE — Evidence & Resilience Model α0.1

## Purpose

TSUGARU GUIDE is not only a guide interface. It can also function as an observation loop for regional support programs, especially tourism and relocation-experience support.

The objective is to preserve evidence of how people encounter the local landscape, what changes in their understanding, what barriers appear, and what remains unresolved.

## Core Loop

```text
Traveler Context
      ↓
Journey Experience
      ↓
Observed Change
      ↓
Evidence
      ↓
Observation
      ↓
Protocol / Service Revision
      ↓
Next Journey
```

The unit of value is not simply `relocated = true`. A journey that clarifies why a person does not fit the local living landscape is also a meaningful outcome.

## Evidence Model

Evidence SHOULD preserve:

- source
- timestamp
- context in which it was observed
- observed experience
- unresolved questions
- outcome
- provenance

Evidence MUST NOT be rewritten into an unsupported fact.

### Example

```yaml
evidence:
  outcome: relocation_not_pursued
  observation:
    - winter_mobility_created_anxiety
    - available_work_options_did_not_match_expectation
  interpretation:
    - strengthen_pre_arrival_winter_life_information
  provenance:
    source: participant_reflection
    timestamp: 2026-08-26
```

A negative or non-conversion outcome is not automatically a failure. It may be evidence that the support process successfully exposed a mismatch before a costly relocation decision.

## Program Evidence

For a regional support program, the protocol can preserve a longitudinal chain:

```text
Participation
  ↓
Pre-journey Context
  ↓
Experience
  ↓
Observed Change
  ↓
Post-journey Context
  ↓
Decision / Non-decision
  ↓
Later Outcome (when available)
```

This supports evaluation of *how* a service contributed to a change, rather than relying only on counts such as participants, consultations, or relocations.

## Resilience

Regional resilience is treated here as the ability of a community and its support system to retain useful experience through change.

The system SHOULD preserve:

- successful patterns
- failed approaches
- unmet needs
- seasonal constraints
- changes in services and infrastructure
- recurring questions from newcomers
- local knowledge that has been verified
- uncertainty where evidence is incomplete

When staff, providers, AI models, or local conditions change, these records can remain available to the next runtime and next generation of guides.

## Governance Boundary

TSUGARU GUIDE MUST NOT become a surveillance or ranking system for residents or applicants.

Personal information SHOULD be minimized. Program evidence SHOULD prefer aggregated, anonymized, or consented records unless a concrete operational purpose requires otherwise.

The guide should support a person's decision; it should not decide whether the person is suitable for the city.

## Architectural Position

```text
Landscape
    ↓
Evidence
    ↓
Protocol
    ↓
Runtime
    ↓
Knowledge / Evidence Adapter
    ↓
AI Persona
```

GitHub is a traceability and knowledge/evidence adapter. It is not the city Landscape itself.

## Future Application

The same model may later support:

- relocation trial programs
- family trial stays
- local business succession support
- community participation programs
- disaster / seasonal resilience observations
- regional service improvement

These are extensions of the same evidence loop, not separate AI products by default.
