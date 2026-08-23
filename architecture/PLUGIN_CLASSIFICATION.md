# Plugin Classification

## Status

Normative architectural classification.

## Purpose

Plugins provide optional, orthogonal capabilities that extend the Runtime without altering Foundation artifacts.

## Categories

- Observation Plugin — captures and normalizes data from external sources or runtime environments.
- Analysis Plugin — consumes observations and produces interpretations or enriched datasets.
- Integration Plugin — bridges external systems and translates between external protocols and Runtime models.
- Rendering Plugin — transforms Runtime outputs into presentation formats or artifacts.

## Principles

- Foundation is immutable.
- Plugins extend Runtime.
- Plugins are independent capabilities.
- Plugins never modify Foundation.

## Boundary

Classification precedes implementation. This specification does not prescribe APIs, loaders, registration, security, configuration, marketplace behavior, or Runtime implementation.
