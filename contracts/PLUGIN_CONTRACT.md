# Plugin Contract

## Status

Normative.

## Purpose

This specification defines the minimum architectural contract for a Shirakami OS plugin.

## Required Metadata

Every plugin MUST declare, at minimum:

- Name
- Version
- Description
- Capability
- Provider

These are conceptual requirements. This specification does not mandate a file layout, serialization format, programming language, or loader implementation.

## Responsibilities

A plugin:

- provides a focused capability;
- extends Runtime behavior without modifying Foundation artifacts;
- operates independently of other plugins;
- is governed by the Runtime.

## Principles

- Single Responsibility
- Loose Coupling
- Runtime Managed
- Foundation Protected

## Out of Scope

APIs, loaders, registration mechanisms, dependency injection, configuration formats, security mechanisms, programming languages, and implementation details are outside this contract.
