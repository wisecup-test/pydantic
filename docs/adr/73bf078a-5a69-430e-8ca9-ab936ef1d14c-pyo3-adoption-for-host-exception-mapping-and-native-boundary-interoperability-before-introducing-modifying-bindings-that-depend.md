# PyO3 Adoption for Host Exception Mapping and Native Boundary Interoperability: Before Introducing Modifying Bindings That Depend

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Native core extension modules require bidirectional interoperability with the host runtime for input validation, serialization, and error surfacing.
- Type mismatch failures occurring deep within validator and serializer routines must be propagated to callers as idiomatic host runtime exceptions without corrupting state.
- Multiple validator, serializer, and error modules require shared access to host exception constructors and string caching mechanisms across foreign function boundaries.

## Problem Statement

Native extension libraries interfacing with dynamic host runtimes face boundary mismatches between native language error structures and host interpreter exception hierarchies. Without a standardized integration framework and consistent exception mapping, boundary routines risk propagating unhandled native panics, duplicating string allocations during key parsing, or raising non-standard exception types that break caller expectations.

## Decision

1. MUST: Before introducing or modifying bindings that depend on pyo3, developers MUST discover the dependency manifest and authoritative repository lock artifact to determine and verify the exact resolved version of pyo3 against official documentation.

## Policy Block

- MUST Before introducing or modifying bindings that depend on pyo3, developers MUST discover the dependency manifest and authoritative repository lock artifact to determine and verify the exact resolved version of pyo3 against official documentation.

In scope:
- Native validation routines executing across the foreign function interface boundary
- Serialization and deserialization handlers interfacing directly with host interpreter objects
- Error conversion layers translating internal validation failures into host exceptions

Out of scope:
- Pure internal native data structures and algorithms with no interaction with the host interpreter
- Isolated utility routines operating strictly on primitive types without runtime crossing

## Rationale

- PyO3 provides mature, type-safe bindings between native code and the host interpreter runtime, minimizing foreign function interface overhead and manual memory management.
- Standardizing on pyo3::exceptions::PyTypeError across serializers and validators ensures consistent exception propagation matching the host runtime standard error hierarchy.
- Utilizing pyo3::intern and pyo3::sync::PyOnceLock minimizes host allocation overhead for repetitive keys and ensures synchronization under interpreter multi-threading.

## Consequences

Positive:
- Uniform and predictable exception propagation to host callers when type validation constraints are violated.
- Reduced host object allocation overhead during key lookup and serialization through string interning.
- Thread-safe lazy initialization of boundary singletons under host interpreter runtime constraints.

Negative:
- Core validation and serialization routines become coupled to the pyo3 crate API surface.
- Instantiating host exception objects introduces boundary crossing overhead relative to pure internal native error propagation.
- Engineers must maintain proficiency in both native memory safety and host interpreter object lifetimes.

## Alternatives

- Manual C foreign function interface bindings using raw interpreter headers (rejected)
  Rejected because: Manual foreign function interface maintenance introduces unsafe pointer manipulations and fragile reference counting prone to memory leaks and process crashes.
  When valid: Valid when targeting minimal embedded environments where high-level binding abstractions cannot be compiled.
- Generic native error propagation converted only at top-level module export wrappers (rejected)
  Rejected because: Delaying host exception conversion to top-level exports obscures granular line errors and type context captured within specific validation and serialization units.
  When valid: Valid in simple monolithic wrappers where intermediate context preservation is unnecessary.

## Risks

- Upstream changes in pyo3 binding APIs or exception handling mechanics across releases could require widespread updates across validation and serialization modules.
  Mitigation: Encapsulate boundary conversions within dedicated error conversion routines and verify locked dependency versions before upgrades.
  Owner: Core Engineering Team
- Excessive or misplaced interning calls on highly dynamic strings could lead to unbounded host memory retention.
  Mitigation: Restrict pyo3::intern usage to statically known schema keys, field names, and fixed literal identifiers.
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
- Ensure all type mismatch paths in validation and serialization routines instantiate pyo3::exceptions::PyTypeError via established helper functions to preserve structured error details.
- Employ pyo3::sync::PyOnceLock for singleton constructs requiring lazy evaluation under the host interpreter lifecycle.

## Continuation Context


Verify commands:
- Discover and run the project test suite via the repository build tool to verify exception propagation and boundary conversions.
- Discover and execute the repository static analysis and linting scripts to verify compliance with binding conventions and safety constraints.

Accept when:
- All boundary validation and serialization tests pass with invalid input types correctly surfacing as host type errors.
- No unhandled native panic occurs across any foreign function interface boundary.
- Static verification and repository linting workflows complete without errors.

## Enforcement

- Verified by: Automated continuous integration testing across all supported target platforms.
- Verified by: Peer code review for all pull requests modifying foreign function interface boundaries.
- Verified by: Repository static analysis and binding linter checks executed during merge validation.
- Violation handling: Pull requests introducing non-standard boundary error mappings or unvalidated binding calls are blocked from merging.
- Violation handling: Detected memory leaks or unhandled boundary panics trigger immediate remediation before release.
- Exception process: Exceptions for alternative exception types or raw pointer manipulation require an architectural review request and explicit approval from maintainers.
- Exception process: Approved exceptions must document isolation guarantees and supply automated test coverage verifying interpreter stability.