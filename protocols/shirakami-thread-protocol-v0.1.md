# Shirakami Thread Protocol v0.1

## Status

**Draft specification / Phase 0 simulation-tested**

This protocol defines a thread-oriented interaction boundary that can operate as an application-level protocol within the Shirakami architecture.

It does not replace the Shirakami Model, Runtime, Evidence, or Human Gate.

## Purpose

Shirakami Thread Protocol reproduces a lightweight thread-style dialogue structure in which:

- conversation advances as discrete Posts;
- text, ASCII art (AA), and silence may coexist as interaction signals;
- conversation can be released rather than allowed to accumulate indefinitely;
- cooling is represented as an explicit interaction state;
- AI remains a simulator and interaction environment, not an authority.

## Architectural Position

The Thread Protocol is an application-level protocol layered on the existing Shirakami stack.

```text
Landscape
   ↓
Context
   ↓
Thread Protocol
   ↓
Runtime / AI Adapter
   ↓
Observation
   ↓
Evidence
   ↓
Human Gate
```

The Thread Protocol MUST NOT bypass the Evidence or Human Gate boundaries.

Provider-specific AI behavior MUST remain distinguishable from Thread Protocol behavior.

## Thread Lifecycle

```text
Thread Start
    ↓
Post Exchange
    ↓
Optional CoolDown
    ↓
Thread Close
    ↓
Release
```

A closed thread MUST NOT be treated as an active authority or continuing relationship.

## Post Contract

Each Post has the following logical fields:

```yaml
post:
  id: "<stable or runtime-generated identifier>"
  author: "User | Shirakami"
  content: "<text | AA | mixed>"
  mode: "Normal | CoolDown | Silence"
  metadata:
    protocol_version: "0.1"
```

Emotion is intentionally not a required normative field.

If an implementation derives an emotion indicator, it MUST be treated as an interpretation of the Post rather than verified user state.

## AA Contract

AA is an optional expressive signal.

AA MAY communicate:

- tone;
- rhythm;
- emphasis;
- reaction;
- conversational spacing;
- cultural or playful context.

AA MUST NOT, by itself, be treated as:

- a clinical measurement;
- a psychological diagnosis;
- verified emotional state;
- evidence of user intent.

The implementation SHOULD preserve the distinction between:

```text
AA / expression
      ↓
contextual interpretation
      ↓
response behavior
```

rather than:

```text
AA
 ↓
asserted psychological state
```

Users MUST be able to participate without using AA.

## Silence and CoolDown

Silence is a protocol state, not necessarily an empty message.

Because ordinary chat interfaces may reject an empty message, an implementation MAY represent soft silence using a minimal token such as `…`, while retaining the canonical state as:

```text
Silence / CoolDown
```

CoolDown is a safety and interaction-control mechanism.

It MUST NOT be presented as a clinical assessment.

A CoolDown transition MAY occur when the implementation detects conditions requiring reduced interaction intensity. The detection rule is implementation-specific and MUST NOT be represented as verified psychological fact.

## Human Control

The Thread Protocol MUST preserve user agency.

The implementation MUST NOT:

- claim authority over the user's decisions;
- infer a user's preference from AA alone;
- pressure the user to continue a thread;
- manufacture emotional dependency;
- convert an interpretation into Evidence;
- prevent the user from closing or leaving a thread.

The AI may respond, pause, or close according to the protocol, but the protocol does not transfer decision authority from the human.

## Thread Closing

An implementation MAY define automatic closing conditions such as:

- post-count threshold;
- elapsed interaction time;
- explicit user close;
- CoolDown completion;
- safety boundary.

The original Phase 0 simulation used **10 Posts or 30 minutes** as a test condition.

These values are experimental parameters, not normative requirements of v0.1.

## Evidence Boundary

Thread Runtime output is an **Observation**.

It becomes Evidence only through the existing Shirakami Evidence path.

```text
Thread Runtime
    ↓
Observation
    ↓
EvidenceRecord
    ↓
stable evidence_id
    ↓
Semantic Handoff
    ↓
Human Gate
```

A Thread implementation MUST NOT assign Evidence status merely because an AI produced a response.

## Compatibility with Shirakami

The Thread Protocol is compatible with the existing project architecture when:

1. the Thread is treated as a Protocol boundary rather than a new AI model;
2. Runtime output remains distinguishable from Evidence;
3. Evidence receives stable identifiers through the existing Evidence boundary;
4. Semantic Handoff carries the relevant context without transferring decision authority;
5. Human Gate remains the final decision boundary;
6. provider-specific behavior remains replaceable.

## Reviewer Path

A reviewer can test the Thread Protocol through:

```text
Landscape
  ↓
Thread Protocol
  ↓
Runtime
  ↓
Observation
  ↓
Evidence
  ↓
Verification
  ↓
Human Gate
```

The protocol is therefore reviewable without requiring the reviewer to accept the protocol's claims in advance.

## Phase 0 Findings

The initial simulation demonstrated:

- Thread Start → Post exchange → Thread Close;
- normal text conversation within Post boundaries;
- AA-only and mixed AA/text Posts;
- changes in conversational rhythm using AA;
- soft silence using `…`;
- explicit CoolDown representation;
- automatic closing after the test Post limit.

The simulation also exposed implementation risks:

- AI may over-interpret AA;
- AI may drift into poetic or psychological framing;
- AI may ask unnecessary follow-up questions;
- AI may prematurely close a test interaction;
- empty-message silence is not available in ordinary chat UI;
- numerical emotion/heat values require an independently defined measurement contract.

These findings are observations from Phase 0, not normative claims about users or clinical outcomes.

## Non-Goals

This protocol does not:

- create a new LLM;
- define a clinical intervention;
- diagnose or assess users;
- make psychological claims from AA;
- replace the Shirakami Evidence Protocol;
- replace Semantic Handoff;
- replace Human Gate;
- define provider-specific API behavior;
- require a particular AI vendor.

## Implementation Guidance

A minimal implementation can expose:

```text
start_thread()
append_post()
render_post()
transition_mode()
request_close()
close_thread()
emit_observation()
```

The implementation MAY begin as CLI-only.

UI, image assets, audio, and provider-specific adapters are optional layers and MUST NOT alter the normative Thread contract.

## Relationship to Other Shirakami Protocols

The Thread Protocol should be used together with the existing project-level protocols:

- Reviewer Protocol — review and verification boundary;
- Evidence Protocol — evidence classification and provenance;
- Semantic Handoff — cross-boundary meaning transfer;
- Runtime protocols — execution boundary.

The Thread Protocol adds a **conversation lifecycle and interaction representation layer**. It does not redefine those existing boundaries.

## Design Principle

> A thread should let conversation flow without turning conversation into authority.

And, consistent with the Shirakami project:

> AI is a simulator, not an authority.
