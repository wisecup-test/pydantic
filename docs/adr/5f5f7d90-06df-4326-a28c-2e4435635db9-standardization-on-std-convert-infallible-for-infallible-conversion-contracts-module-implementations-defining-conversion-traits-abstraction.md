# Standardization on std::convert::Infallible for Infallible Conversion Contracts: Module Implementations Defining Conversion Traits Abstraction

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Internal abstractions across input processing, error handling, lookup path resolution, and serialization require trait implementations that declare associated error types.
- Certain data conversion and extraction routines cannot fail at runtime because input invariants are guaranteed by upstream contracts.
- Using custom empty error enums or fallible error types for infallible operations introduces unnecessary branching, increases maintenance overhead, and obscures infallibility guarantees.
- Standardizing on an uninhabited error type from core conversion primitives enables the compiler to verify and optimize infallible code paths across module boundaries.

## Problem Statement

Trait-based conversions and abstractions across core processing modules often require associated error types even when the underlying conversion is guaranteed to succeed. Without a consistent convention, modules risk defining redundant error types or using general fallible error types that force downstream consumers to write redundant error-handling routines for impossible failure states.

## Decision

1. MUST: Module implementations defining conversion traits or abstraction interfaces where operations cannot fail MUST specify std::convert::Infallible as the associated error type.

## Policy Block

- MUST Module implementations defining conversion traits or abstraction interfaces where operations cannot fail MUST specify std::convert::Infallible as the associated error type.

In scope:
- Trait implementations, conversion adapters, and abstract input interfaces within core processing modules where operations cannot fail.
- Internal data transformation boundaries requiring static proof of infallibility.

Out of scope:
- Operations that involve dynamic runtime validation, parsing external untrusted input, or input/output operations that can fail.
- External boundary contracts where foreign callers explicitly expect runtime error values.

Exceptions:
- EXC-20-001: An upstream or third-party trait contract strictly mandates an explicit foreign error type that cannot be unified with uninhabited error types.

## Rationale

- Adopting std::convert::Infallible establishes an unambiguous contract that an operation is guaranteed never to fail, allowing the type system to enforce correctness.
- Using a standardized uninhabited type prevents proliferation of redundant, module-specific empty error enums across internal packages.
- Infallible error types enable the compiler to perform dead-code elimination and optimize away error handling branches across call sites.

## Consequences

Positive:
- Eliminates defensive error handling and unreachable error branches in consuming code.
- Provides compile-time guarantees of operation success across abstraction boundaries.
- Unifies error type representation for infallible conversions across serializers, input extractors, and lookup routines.

Negative:
- Adapting infallible operations to external interfaces requiring specific foreign error types requires explicit conversion mapping layers.
- Refactoring an infallible operation into a fallible operation in the future constitutes a breaking API contract change.

## Alternatives

- Define bespoke uninhabited error types independently within each module requiring infallible conversions. (rejected)
  Rejected because: Creates redundant type definitions that cannot be unified across module boundaries and increases maintenance overhead.
  When valid: When an isolated component cannot depend on standard library conversion primitives.
- Use generalized fallible error enums with unused error variants for infallible operations. (rejected)
  Rejected because: Forces call sites to implement unnecessary error handling routines and prevents compiler optimization of dead code paths.
  When valid: When an interface contract is scheduled to become fallible in a planned revision.

## Risks

- Future requirements may introduce potential failure points to an operation previously guaranteed to be infallible.
  Mitigation: Design distinct trait methods or versioned interfaces when introducing fallible processing to avoid breaking existing infallible contracts.
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
- When implementing conversion traits for types guaranteed to convert without error, declare the associated error type as std::convert::Infallible.
- Ensure downstream pattern match expressions take advantage of the uninhabited nature of the error type to eliminate runtime checks.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and compilation check directives defined in the repository configuration.
- Execute the primary project test suite according to the repository test configuration to confirm type checking and conversion behavior.

Accept when:
- All conversion trait implementations for infallible operations compile successfully with std::convert::Infallible as their associated error type.
- Repository static analysis and test validation suites pass without type mismatch errors or unhandled error warnings.

## Enforcement

- Verified by: Automated continuous integration build checks and compiler type validation.
- Verified by: Peer code review on all pull requests modifying conversion trait implementations or error types.
- Violation handling: Pull requests introducing ad-hoc empty error types or fallible signatures for infallible conversions will be blocked during review.
- Violation handling: Type errors resulting from mismatched error signatures will fail automated compilation checks in continuous integration.
- Exception process: Exceptions require approval from core maintainers documented in the pull request and within the relevant module implementation.