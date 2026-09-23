# Standardized Schema Error Construction via crate::build_tools::py_schema_err: Developers Discover Project Dependency Manifest Lock

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Schema construction and configuration parsing routines across validators and serializers transform foreign-function interface definitions into internal execution plans.
- Configuration defects, unsupported types, or malformed schemas detected during setup require distinct error representation separating compile-time structural faults from runtime data validation errors.
- Multiple modules across the core serialization and validation subsystems must report schema compilation failures through a uniform exception mapping mechanism without duplicating error construction logic.

## Problem Statement

When parsing schema dictionaries and configuration inputs across validator and serializer builder routines, invalid configurations or unsupported definitions must be converted into user-facing schema errors. Without a standardized internal error constructor, distinct builder components risk introducing inconsistent error types, divergent diagnostic messages, or leaking runtime validation error structures into schema compilation phases.

## Decision

1. MUST: Developers MUST discover the project dependency manifest and lock artifact to inspect the exact resolved dependency versions before implementing or modifying interfaces interacting with external foreign-function interface bindings.

## Policy Block

- MUST Developers MUST discover the project dependency manifest and lock artifact to inspect the exact resolved dependency versions before implementing or modifying interfaces interacting with external foreign-function interface bindings.

In scope:
- Schema parsing and configuration decoding across validator builders
- Serializer initialization and type serializer builder routines
- Configuration mapping and option parsing during schema compilation

Out of scope:
- Runtime input data validation error generation
- Runtime serialization formatting and state serialization errors

Exceptions:
- EX-20-001: An underlying serialization engine produces a native formatting error during runtime serialization execution

## Rationale

- Enforcing crate::build_tools::py_schema_err across all validator and serializer builders ensures consistent schema error creation and uniform diagnostic formatting across the entire engine.
- Decoupling schema compilation errors from runtime validation errors isolates schema configuration defects from operational input validation failures.
- Centralizing schema error generation prevents code duplication across independent builder routines and guarantees predictable error propagation across the foreign-function boundary.

## Consequences

Positive:
- Uniform error reporting guarantees consistent exception types and diagnostic messages for invalid schema definitions across all builders.
- Strict separation between schema setup errors and runtime validation errors clarifies failure domains for consumers.
- Shared helper utilization eliminates duplicated error conversion boilerplate across serialization and validation builder implementations.

Negative:
- Builder routines depend directly on the shared internal build tools module rather than constructing localized error types.
- Error message formatting remains constrained by the signature and conventions of the central schema error constructor.

## Alternatives

- Ad-hoc exception construction directly within each builder module (rejected)
  Rejected because: Leads to inconsistent exception types, fragmented error message conventions, and high boilerplate duplication across modules.
  When valid: Valid only in standalone prototypes without cross-cutting schema builder coordination.
- Reusing runtime validation error structures for schema construction failures (rejected)
  Rejected because: Conflates developer schema configuration mistakes with runtime user data validation errors, producing misleading diagnostics.
  When valid: Valid in architectures where schema definitions are themselves validated as standard runtime data instances.

## Risks

- Changes to the signature or behavior of crate::build_tools::py_schema_err propagate across all builder and serializer modules
  Mitigation: Maintain strict backward compatibility in the helper module interface and enforce unit testing across schema error generation paths
  Owner: Core Engine Team
- Developers might inadvertently throw runtime validation errors during schema construction if guidelines are not observed
  Mitigation: Verify schema construction return types through compile-time type checking and automated code review
  Owner: Core Engine Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When adding new validators or serializers, import crate::build_tools::py_schema_err to handle missing, invalid, or mutually exclusive schema options.
- Combine schema error construction with pyo3::intern for key lookup to optimize schema parsing without compromising error specificity.

## Continuation Context


Verify commands:
- Discover and execute the repository compilation command to ensure all schema builders adhere to error type contracts.
- Discover and run the project test suite covering schema compilation and invalid schema error emission.
- Discover and execute the project linter to check that no direct unformatted exception constructors bypass the shared build tools helper.

Accept when:
- All schema compilation failures across validators and serializers yield the expected schema error exception.
- Project compilation and automated test suites pass without unresolved error construction mismatches.
- Code review confirms new and modified builders employ crate::build_tools::py_schema_err for invalid schema scenarios.

## Enforcement

- Verified by: Continuous integration test suites verifying schema error emissions
- Verified by: Static type analysis and compiler checks confirming error return types
- Verified by: Peer code review for all new validator and serializer implementations
- Violation handling: Pull requests utilizing direct exception creation or runtime validation error types for schema errors must be blocked until refactored to use crate::build_tools::py_schema_err.
- Exception process: Exceptions require documented technical justification and approval from core library maintainers via formal code review.