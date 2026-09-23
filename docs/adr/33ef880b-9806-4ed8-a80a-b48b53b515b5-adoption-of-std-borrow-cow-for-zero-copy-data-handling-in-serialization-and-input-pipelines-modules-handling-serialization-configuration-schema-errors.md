# Adoption of std::borrow::Cow for Zero-Copy Data Handling in Serialization and Input Pipelines: Modules Handling Serialization Configuration Schema Errors

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- High-throughput data serialization and input processing frequently handle textual and structured data across foreign function interfaces.
- Unconditional allocation and cloning of strings and byte slices during serialization introduces substantial heap churn and performance degradation.
- Static analysis across type serializers and input handling modules reveals widespread adoption of clone-on-write smart pointers to manage borrowed versus owned memory states.

## Problem Statement

High-frequency data validation and serialization paths require flexible memory management that can reference borrowed data when available but allocate owned buffers when transformations occur. Standardizing on owned string allocations degrades performance, while rigid borrowing creates lifetime friction across module boundaries.

## Decision

1. MUST: Modules handling serialization configuration and schema errors MUST allocate Cow::Owned variants only when string mutation, value encoding, or runtime generation requires taking ownership.

## Policy Block

- MUST Modules handling serialization configuration and schema errors MUST allocate Cow::Owned variants only when string mutation, value encoding, or runtime generation requires taking ownership.

In scope:
- Type serialization routines processing string, formatted, or collection values
- Input processing routines extracting and returning enum, string, or boolean representations
- Configuration mapping and serialization state components handling dynamically encoded outputs

Out of scope:
- Primitive scalar types that implement copy semantics without heap allocation
- Internal definitions builders storing fixed structural identifiers with static lifetimes

Exceptions:
- EXC-20-001: External interface contracts require an unconditionally owned string or buffer due to foreign function boundary memory ownership transference

## Rationale

- Adopting std::borrow::Cow enables zero-copy deserialization and serialization wherever input data matches expected target representations.
- Clone-on-write semantics defer memory allocation until modification or reformatting is strictly necessary, optimizing common read-heavy paths.
- The pattern integrates cleanly with thread synchronization primitives and shared definitions builders across the serializer subsystem.

## Consequences

Positive:
- Reduces heap allocations across string, date-time, uuid, and collection serialization pipelines.
- Maintains ergonomic API contracts that accommodate both transient references and newly constructed owned strings.
- Optimizes cross-runtime data passing by avoiding memory duplication when inputs can be directly inspected.

Negative:
- Increases function signature complexity by exposing lifetime annotations and wrapper variants.
- Requires developers to reason about lifetime bounds when passing borrowed values across closure and thread boundaries.
- May introduce minor branching overhead at runtime to inspect whether a variant is borrowed or owned.

## Alternatives

- Unconditional owned string allocation across all serialization and input routines (rejected)
  Rejected because: Incurs excessive heap allocation overhead on high-throughput serialization workloads where most data is unmodified
  When valid: Viable only in low-performance environments where code simplicity outweighs memory and latency budgets
- Strict borrowed reference slices across all serialization interfaces (rejected)
  Rejected because: Prevents serializers from applying dynamic transformations, formatting, escaping, or fallback serialization that requires ownership
  When valid: Viable only for strict zero-copy parsers that never modify, escape, or synthesize output representations

## Risks

- Accidental conversion from borrowed to owned variants during chained operations, negating performance benefits
  Mitigation: Establish code review standards and benchmark suites to detect unintended allocations in hot paths
  Owner: engineering team
- Complex lifetime propagation causing compiler errors when refactoring nested serializer definitions
  Mitigation: Encapsulate complex lifetime interactions within dedicated helper abstractions and documented serializer contracts
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Inspect method return types across serializers to ensure borrowed variants are emitted unless mutations or formatting occur.
- Coordinate clone-on-write pointers with shared definitions builders and state containers to prevent redundant reference duplication.

## Continuation Context


Verify commands:
- Discover the repository test runner from the project manifest and execute the test suite covering type serializers.
- Discover and run the project linter and static analysis checks to ensure compliance with lifetime and memory management guidelines.
- Discover and execute benchmark suites to measure allocation behavior across serialization workloads.

Accept when:
- The project test suite passes completely without memory safety or lifetime violations.
- Static analysis and linting scripts report zero errors across serialization and input processing modules.
- Performance benchmarks confirm zero-copy paths avoid heap allocations during unescaped serialization.

## Enforcement

- Verified by: Continuous integration test suites and static analysis checks
- Verified by: Peer code review evaluating memory allocation patterns and lifetime annotations
- Violation handling: Pull requests introducing unconditional allocations in hot paths will be blocked pending optimization or justification
- Exception process: Submit an architectural review request documenting why ownership transference is required for foreign function boundaries