# ThreadRPG Reference Form v1.0

## Status

Reference-form implementation specification.

ThreadRPG is the narrative and conversational reference form from which Shirakami Architecture is extracted. It is not an AI model and does not grant authority to Runtime or an AI provider.

## Reference origin

- Origin: `白神高校ブラスバンド部`
- Unit of continuity: observation Thread
- Central object: shared Landscape
- Authority for Catch: human

## Required behavior

A conforming implementation MUST preserve the following boundaries:

1. **Landscape**
   - Observations refer to one shared Landscape.
   - The implementation MUST preserve the current Landscape state separately from individual observations.

2. **Observation First**
   - Observation precedes answer/closure.
   - The implementation MUST allow an observation to remain unresolved.

3. **Seven viewpoints**
   - The default reference form exposes seven observation points.
   - Viewpoints are positions of observation, not fixed expert identities or independent authorities.
   - A conforming implementation MUST NOT require seven fixed personalities.

4. **Thread**
   - A Thread is a temporal connection between observations.
   - Previous observations MUST remain addressable when a new observation is added.
   - New observations MUST NOT silently overwrite prior observations.

5. **Dark Layer**
   - Unresolved questions MAY be held without resolution.
   - The system MUST NOT fabricate a resolution merely to empty the unresolved set.

6. **Revisit**
   - A held question can be re-observed when time or new context provides a trigger.
   - Revisit creates a new observation; it does not rewrite the prior observation.

7. **Rainwater Mode**
   - Rainwater Mode increases revisit/connection pressure on unresolved material.
   - It MUST NOT force resolution.

8. **Catch / Human Gate**
   - The system may surface observations and connections.
   - Only the human may declare a Catch.
   - Runtime, AI, renderer, or protocol execution MUST NOT declare Catch as an authoritative decision.

## Evidence boundary

A conforming Runtime integration SHOULD expose state transitions as inspectable Evidence.

Evidence MAY record:
- observation creation,
- unresolved-question creation,
- revisit,
- rainwater-mode activation,
- human Catch declaration.

Evidence MUST distinguish observation from authorization.

## Completion boundary

ThreadRPG Reference Form v1.0 is complete when:

- all eight required behaviors above are represented by executable state transitions;
- prior observations remain preserved;
- unresolved questions remain unresolved until human action or later observation;
- revisit creates new observations;
- rainwater mode does not resolve questions;
- Catch is human-only;
- tests cover each boundary;
- the implementation can run without a specific AI provider.

## Non-goals

This specification does not require:
- a particular LLM,
- seven fixed AI personas,
- automatic correctness,
- automatic decision-making,
- a graphical UI,
- a specific renderer,
- a particular backend.
