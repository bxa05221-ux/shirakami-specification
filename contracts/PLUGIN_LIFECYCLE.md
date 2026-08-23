# Plugin Lifecycle Contract

## Status

Normative.

## Lifecycle States

A plugin may occupy the following Runtime-managed states:

1. Installed — artifacts and metadata are present but the Runtime has not yet acknowledged the plugin for operation.
2. Registered — the Runtime has recorded the plugin metadata; registration does not imply activation.
3. Activated — Runtime preconditions are satisfied and execution resources may be prepared.
4. Running — the plugin is actively performing its declared responsibility.
5. Suspended — execution is temporarily paused while state is preserved for possible resumption.
6. Stopped — execution has ended and Runtime resources have been released.
7. Removed — plugin artifacts and registration are no longer recognized by the Runtime.

## Principles

- Runtime controls lifecycle transitions.
- Foundation remains immutable.
- Plugins do not manage other plugins.

## Boundary

This contract defines lifecycle semantics only. It does not prescribe APIs, loaders, dependency resolution, security, configuration, persistence, or implementation details.
