# RFC → Specification Migration Map

## Status

Normative migration record.

This document records the relationship between the original RFC documents in `shirakami-OS/docs/rfc/` and their promoted specifications in this repository.

| Source RFC | Specification | Status |
|---|---|---|
| RFC-0001 Plugin Classification | `architecture/PLUGIN_CLASSIFICATION.md` | Promoted |
| RFC-0002 Plugin Lifecycle | `contracts/PLUGIN_LIFECYCLE.md` | Promoted |
| RFC-0003 Runtime Lifecycle | `contracts/RUNTIME_LIFECYCLE.md` | Promoted |
| RFC-0004 Plugin Contract | `contracts/PLUGIN_CONTRACT.md` | Promoted |
| RFC-0005 Contract Layer Overview | `architecture/CONTRACT_LAYER_OVERVIEW.md` | Promoted |
| RFC-0006 Runtime Interface Contract | — | Draft / retained in `shirakami-OS` |

## Migration Principle

Promotion does not erase provenance. The original RFC remains the historical source document until an explicit repository migration or archival decision is made.

The promoted specification is the normative location for future references. The implementation repository may retain the original RFC temporarily to preserve history and transition context.

## Evidence

The promotion establishes a concrete separation between:

- exploratory or historical RFC material;
- normative specifications;
- executable Runtime implementation.

Future changes to a promoted specification should be made here and should preserve a traceable relationship to implementation changes in `shirakami-OS`.
