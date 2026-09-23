# Standardization on crate::serializers::SerializationState for Serializer Context Management: Serializer Implementations Within Serialization Subsystem Accept

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- The serialization engine requires coordinated tracking of traversal depth, formatting preferences, recursive definitions, and runtime serialization configurations across diverse data types.
- Passing disparate serialization parameters independently across deeply nested serializer call hierarchies causes signature bloat and inconsistencies across type serializers.
- Analysis across serializer components reveals uniform threading of crate::serializers::SerializationState to provide a unified runtime execution context during serialization passes.

## Problem Statement

Without a standardized context structure, type-specific serializers require independent parameter passing for serialization flags, recursion guards, and format overrides, leading to brittle signatures and inconsistent error handling across serializer routines.

## Decision

1. MUST: All serializer implementations within the serialization subsystem MUST accept and thread crate::serializers::SerializationState through serialization execution paths to maintain consistent execution context.

## Policy Block

- MUST All serializer implementations within the serialization subsystem MUST accept and thread crate::serializers::SerializationState through serialization execution paths to maintain consistent execution context.

In scope:
- Implementation of schema serializers, prebuilt serializers, and type serializers within the serialization subsystem.
- Type-specific serialization logic requiring traversal state, recursion control, or configuration flags.

Out of scope:
- Stateless transformation utilities that do not participate in serialization schema traversal or context propagation.
- Deserialization or input validation workflows governed by independent validation contexts.

Exceptions:
- EXC-20-001: A leaf serializer performs constant, stateless primitive emission with no traversal or contextual configuration requirements.

## Rationale

- Evidence from eight serializer implementations demonstrates that threading crate::serializers::SerializationState ensures deterministic serialization behavior across diverse data representations.
- Encapsulating traversal state and runtime options within a single context object minimizes function signature churn when new serialization options are introduced.
- Centralizing context in crate::serializers::SerializationState prevents scattered state mutations and simplifies tracking recursion and cycle limits during complex schema traversal.

## Consequences

Positive:
- Uniform serialization signatures across all type serializers simplify maintenance and modular extensions.
- Consistent propagation of serialization configurations guarantees identical formatting behavior across nested type hierarchies.
- Centralized recursion detection and state tracking reduce the likelihood of stack overflow defects during cyclic structure traversal.

Negative:
- Couples all type serializers to the internal interface and lifecycle of crate::serializers::SerializationState.
- Requires threading the state argument through every intermediate serialization helper function even when leaf transformations do not inspect it directly.

## Alternatives

- Thread-local or global runtime state for serialization parameters and recursion tracking (rejected)
  Rejected because: Global or thread-local state creates concurrency hazards, complicates re-entrant serialization, and hinders deterministic isolation between concurrent serialization operations.
  When valid: Valid only in single-threaded legacy runtimes where function signatures cannot be modified.
- Individual explicit function arguments for every serialization flag, recursion counter, and configuration setting (rejected)
  Rejected because: Results in excessive parameter list expansion across all serializer functions and requires modifying every serializer signature whenever a new serialization option is added.
  When valid: Valid in minimal systems with strictly fixed, non-configurable serialization formats.

## Risks

- Performance overhead from accessing or updating complex context structures inside tight serialization loops.
  Mitigation: Design crate::serializers::SerializationState with minimal indirection and favor lightweight borrow patterns during read-heavy operations.
  Owner: Core Engineering Team
- Accidental state leakage between independent serialization passes if state instances are improperly reused.
  Mitigation: Enforce instance-per-root-traversal lifecycles in top-level serializer dispatch entrypoints.
  Owner: Core Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When defining new type serializers, implement serialization methods to receive crate::serializers::SerializationState as an active parameter and pass it into child serializer calls.
- Coordinate definitions resolution by integrating DefinitionsBuilder during schema preparation to ensure type serializers resolve recursive identifiers through crate::serializers::SerializationState.

## Continuation Context


Verify commands:
- Discover the test runner from the repository configuration and execute unit and integration suites targeting serialization modules.
- Discover the repository static analysis and linting verification tasks to ensure all serializer signatures adhere to state passing rules.

Accept when:
- All serializer test suites execute successfully with no regression in serialization performance or correctness.
- Static analysis verification completes with zero warnings regarding inconsistent serialization state arguments.

## Enforcement

- Verified by: Automated test suites executed in continuous integration verifying serializer output consistency across nested types.
- Verified by: Static analysis checks verifying function signature conformance across serializer modules.
- Verified by: Peer review on pull requests touching serializer definitions.
- Violation handling: Pull requests introducing serializers that bypass crate::serializers::SerializationState will fail automated checks and review approval.
- Violation handling: Non-compliant signatures must be refactored to thread the standard state object prior to merge.
- Exception process: Submit an architectural review request demonstrating why state-free serialization is required for the specific leaf serializer.
- Exception process: Document approval and rationale within the relevant module documentation.