# Adoption of pydantic_core.core_schema for Core Schema Representation and Traversal: Deferred Discriminator Schemas Identified During Core

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- The framework requires a unified intermediate schema definition language to bridge high-level declarative model definitions with low-level validation engine execution.
- Direct dependency on pydantic_core.core_schema establishes typed contracts for core schemas, definition references, and schema metadata across internal processing routines.
- Centralizing schema traversal, definition inlining, and discriminator deferral on standardized core schema representations prevents divergent schema representations across the codebase.

## Problem Statement

Managing declarative schema representations without a shared structural contract leads to inconsistent traversal logic, fragile definition reference resolution, and tight coupling across compilation stages. The framework requires a standardized schema data model to inspect, gather, and clean schema definitions across internal subsystems and package entry points.

## Decision

1. SHOULD: Deferred discriminator schemas identified during core schema traversal SHOULD be collected into dedicated sequence buffers within the traversal context rather than evaluated eagerly during schema gathering.

## Policy Block

- SHOULD Deferred discriminator schemas identified during core schema traversal SHOULD be collected into dedicated sequence buffers within the traversal context rather than evaluated eagerly during schema gathering.

In scope:
- Internal modules performing schema traversal, reference collection, or schema cleaning.
- Package entry points exporting core schema types and dynamic attribute resolution hooks.

Out of scope:
- User-facing high-level model declaration interfaces that do not directly inspect or manipulate core schemas.
- External serialization format adapters operating strictly on pre-compiled schema artifacts.

## Rationale

- Adopting pydantic_core.core_schema provides an authoritative structural contract for core schemas and definition references, ensuring complete alignment with the underlying validation runtime.
- Encapsulating traversal logic within typed gathering contracts like GatherResult guarantees uniform collection of definition references and deferred discriminator schemas.
- Centralizing core schema representations minimizes duplicated schema parsing logic and ensures consistent handling of missing definitions through structured lookup exceptions.

## Consequences

Positive:
- Establishes a single, strongly-typed schema intermediate representation across internal traversal and cleaning workflows.
- Eliminates duplicate schema traversal logic by centralizing definition reference collection and discriminator deferral.
- Guarantees direct compatibility between higher-level model inspection routines and the validation engine core schema format.

Negative:
- Couples internal schema traversal directly to the schema specification of the external core library.
- Requires schema traversal routines to handle complex union and recursive definition reference graph structures.

## Alternatives

- Define independent internal AST classes for schema traversal and map to core schemas only at final validation emission (rejected)
  Rejected because: Introducing an intermediary schema AST introduces redundant conversion overhead and maintenance burden when core schema specifications evolve.
  When valid: Valid when the underlying execution engine lacks standardized schema typing definitions.
- Perform schema gathering and cleaning directly on unvalidated nested dictionary structures without typed traversal contracts (rejected)
  Rejected because: Unstructured dictionary manipulation increases runtime errors and obscures schema contract requirements across compilation stages.
  When valid: Valid in lightweight prototypes with flat, non-recursive schema structures.

## Risks

- Upstream changes to core schema structure definitions could break internal traversal and reference resolution routines.
  Mitigation: Adhere to the lock-version grounding policy and validate traversal contracts against exact locked core library versions.
  Owner: Core Framework Engineering Team
- Recursive schema reference graphs may cause infinite recursion or reference resolution failure during schema gathering.
  Mitigation: Enforce visited reference tracking and explicit lookup error handling for unresolved schema definitions.
  Owner: Core Framework Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Traverse core schemas recursively using dedicated traversal callbacks for schema definitions, references, and metadata dictionaries.
- Maintain schema reference definitions within a dedicated context mapping to distinguish between inlinable single-reference schemas and shared definitions.

## Continuation Context


Verify commands:
- sh -c 'test -n "$(find . -maxdepth 3 -type f \( -name "*test*" -o -name "*check*" \) | head -n 1)"'
- sh -c 'echo "Discover and run repository verification scripts validating core schema traversal and export contracts"'

Accept when:
- The repository test suite executes successfully with all schema traversal and definition gathering tests passing.
- Schema reference collection accurately identifies inlinable definitions and reports missing definitions without unhandled exceptions.
- All package-level dynamic attribute exports for core schema types resolve without import errors.

## Enforcement

- Verified by: Continuous integration test suite executions validating schema generation and traversal routines.
- Verified by: Automated static type analysis verifying conformance with core schema type definitions.
- Verified by: Peer code review for all modifications touching schema gathering routines or package exports.
- Violation handling: Pull requests introducing unhandled schema references or non-conforming core schema structures are blocked from merging.
- Violation handling: Schema traversal failures trigger automated test failures during verification pipeline runs.
- Exception process: Exceptions for temporary schema traversal shims require written approval from the core framework maintainers and an associated tracking issue for remediation.