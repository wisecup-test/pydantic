# Internal Module Adoption: crate::serializers::SerializationState: When Encountering Unexpected Payload Types During

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Serialization of complex data structures requires passing dynamic execution options, recursion tracking safeguards, and contextual state across nested serializer routines.
- Independent serializer units within the serialization subsystem must maintain a uniform calling convention when traversing input objects and producing transformed representations.
- The internal module crate::serializers::SerializationState serves as the canonical vehicle for carrying runtime serialization state across type serializers, alongside crate::definitions::DefinitionsBuilder for resolving recursive definitions.
- Serialization failures resulting from unexpected data payloads require a standardized error representation across all serializer routines via crate::PydanticSerializationUnexpectedValue.

## Problem Statement

Without a standardized state container across type serializer components, each serializer routine would require individual parameter passing for contextual options and recursion boundaries, creating fragile interfaces, inconsistent state updates, and divergent error propagation during traversal.

## Decision

1. SHOULD: When encountering unexpected payload types during serialization traversal, serializers SHOULD raise crate::PydanticSerializationUnexpectedValue to preserve uniform error telemetry.

## Policy Block

- SHOULD When encountering unexpected payload types during serialization traversal, serializers SHOULD raise crate::PydanticSerializationUnexpectedValue to preserve uniform error telemetry.

In scope:
- Implementation of type serializers within the serialization subsystem
- State coordination routines handling serialization traversal and definition registration

Out of scope:
- Standalone validation routines operating outside serializer execution pathways
- Top-level protocol bindings that do not instantiate or invoke serialization state containers

Exceptions:
- EX-20-001: A serializer requires isolated, stateless serialization without accessing serialization context or definition registries

## Rationale

- Evidence across serializer modules shows consistent coupling to crate::serializers::SerializationState, ensuring a uniform contract for state propagation during traversal.
- Standardizing on crate::serializers::SerializationState and crate::definitions::DefinitionsBuilder prevents parameter drift across serializer signatures and simplifies recursion tracking across complex nested structures.
- Unifying unexpected value handling around crate::PydanticSerializationUnexpectedValue guarantees consistent error formatting and diagnostics across heterogeneous serializer routines.

## Consequences

Positive:
- Consistent interface signatures across all type serializers streamline adding new data type serializers to the subsystem.
- Centralized recursion guards and extra serialization context remain coherent across deeply nested object hierarchies.
- Error handling for unexpected payload values is normalized across distinct type serializers.

Negative:
- Couples all type serializer components to the internal struct definition and lifecycle of crate::serializers::SerializationState.
- Modifications to the shared state management interface require coordinated updates across all implementing serializers.

## Alternatives

- Individual parameter passing for context, recursion depth, and configuration flags across each serializer call (rejected)
  Rejected because: Expanding parameter signatures across dozens of type serializers leads to interface bloat, maintenance overhead, and elevated risk of parameter divergence.
  When valid: Valid only in small standalone serialization utilities with zero recursion and no dynamic runtime configuration.
- Thread-local or global static storage for serialization traversal state (rejected)
  Rejected because: Global or thread-local storage obscures data flow, impairs concurrency predictability, and complicates state cleanup on serialization failure.
  When valid: Valid in legacy runtimes where function signatures cannot be modified to pass state explicitly.

## Risks

- High coupling between serializer routines and crate::serializers::SerializationState could cause cascading compile-time breakages during state container refactoring.
  Mitigation: Encapsulate state fields behind stable accessor methods and preserve backwards-compatible signatures during internal evolution.
  Owner: Core Subsystem Engineering
- Improper recursion counter management within crate::serializers::SerializationState could fail to detect cycles or trigger premature depth limit errors.
  Mitigation: Verify recursion depth guard transitions through comprehensive nested structure unit and regression suites.
  Owner: Serialization Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure type serializers receive and thread crate::serializers::SerializationState references through all child serialization invocations without re-instantiating state containers.
- Cooperate with crate::definitions::DefinitionsBuilder during serializer initialization to resolve forward references and recursive definition identifiers.

## Continuation Context


Verify commands:
- Discover the project's primary test runner from repository configuration and execute the complete test suite covering the serialization subsystem.
- Discover the project's static analysis and linting entry point from repository metadata and run type verification on all serializer implementations.

Accept when:
- All serializer test suites pass without regressions across nested and recursive serialization test cases.
- Type checking and static verification pass with zero warnings across all serializer module definitions.

## Enforcement

- Verified by: Continuous integration pipelines executing repository test suites and static analysis checks on every pull request.
- Verified by: Architectural code review by core subsystem maintainers for all modifications to serializer state signatures.
- Violation handling: Automated pipeline rejection on failing unit tests or incompatible serializer interface definitions.
- Violation handling: Mandatory code revision required prior to merging any pull request that bypasses standard serialization state passing.
- Exception process: Submit an exception request detailing technical necessity and safety analysis to the core subsystem maintainers for review.