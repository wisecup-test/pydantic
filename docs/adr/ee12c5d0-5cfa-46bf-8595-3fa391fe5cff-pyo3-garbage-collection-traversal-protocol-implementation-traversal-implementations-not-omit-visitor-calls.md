# PyO3 Garbage Collection Traversal Protocol Implementation: Traversal Implementations Not Omit Visitor Calls

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Context

- The native core library manages complex validation and serialization pipelines interfacing directly with Python runtime objects.
- Python relies on reference counting augmented by cyclic garbage collection to reclaim memory occupied by isolated reference cycles.
- Rust structures encapsulating Python references risk creating uncollectable circular references across the FFI boundary if hidden from the garbage collector.
- The pyo3 framework provides garbage collection integration primitives including pyo3::PyVisit and pyo3::PyTraverseError to expose internal Python references to the runtime collector.

## Problem Statement

When native Rust components such as serializers, validators, and utility handlers hold references to Python objects, circular references between Rust memory allocations and Python runtime objects cannot be resolved by standard reference counting alone. Without explicit registration and traversal of internal Python references via the pyo3 garbage collection protocol, circular references cause silent memory leaks across the FFI boundary that degrade long-running application performance.

## Decision

1. MUST_NOT: Traversal implementations MUST NOT omit visitor traversal calls for any nested Python runtime references or closures stored within the enclosing native structure.

## Policy Block

- MUST_NOT Traversal implementations MUST NOT omit visitor traversal calls for any nested Python runtime references or closures stored within the enclosing native structure.

In scope:
- Native extension structures within serialization, validation, and tool modules that store or manage Python object references.
- Rust types participating in Python garbage-collected object graphs requiring cyclic reference detection.

Out of scope:
- Pure native data structures that store solely Rust-native primitives and do not retain references to the Python runtime heap.
- Stateless utility functions and pure helper routines that do not persist reference-counted Python objects across call boundaries.

## Rationale

- Direct implementation of the pyo3 cyclic garbage collection traversal protocol ensures that the Python garbage collector can traverse and identify reference cycles originating in native Rust structs.
- Standardizing on pyo3::PyVisit and pyo3::PyTraverseError across serializer, validator, and tool modules establishes a uniform pattern for safe memory reclamation across the language boundary.
- Coordinating traversal protocols with atomic reference counting and lazy initialization primitives guarantees thread safety without compromising cyclic garbage collection mechanics.

## Consequences

Positive:
- Eliminates memory leaks caused by circular references between native Rust structures and Python runtime objects.
- Provides deterministic cyclic garbage collection visibility across the language boundary.
- Enforces a consistent, idiomatic pattern for PyO3 memory management across validation and serialization pipelines.

Negative:
- Imposes boilerplate overhead requiring explicit visitor traversal methods on every struct holding Python references.
- Increases maintenance complexity when refactoring data structures to ensure newly added Python references are included in traversal logic.

## Alternatives

- Rely exclusively on standard Python reference counting without implementing traversal visitors (rejected)
  Rejected because: Reference counting alone fails to detect and collect circular reference cycles across the FFI boundary, leading to persistent memory leaks in complex validation and serialization pipelines
  When valid: Only valid for strictly acyclic object hierarchies containing no mutual references or self-referential closures
- Implement custom manual reference tracking outside the pyo3 traversal framework (rejected)
  Rejected because: Custom memory management bypasses the Python runtime cyclic garbage collector, introduces high defect risk, and duplicates established pyo3 traversal abstractions
  When valid: Valid only in standalone native runtimes operating independently of the Python runtime garbage collector

## Risks

- Omission of nested Python references from visitor implementations during future schema additions can reintroduce silent memory leaks.
  Mitigation: Establish static analysis checks and automated cyclic garbage collection regression tests in the repository test suite.
  Owner: Core Engineering Team
- API surface modifications in pyo3 traversal types across framework version upgrades may break traversal signatures.
  Mitigation: Adhere to the mandatory lock-version grounding policy to verify exact trait contracts against the locked dependency version before upgrading.
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
- When defining native types that hold Python object references or callable serializers, implement the visitor trait by iterating over each encapsulated Python reference and passing it to the visitor callback.
- Ensure error propagation is handled immediately upon visitor callback failure by returning the resulting traverse error to the calling runtime.

## Continuation Context


Verify commands:
- Direct the build system to inspect the project dependency manifest and lock artifact to confirm the declared pyo3 dependency version and traversal feature configuration.
- Execute the repository test suite via the project build tool to verify that cyclic garbage collection tests pass and no memory leaks are detected.
- Run project static analysis checks to verify that every native structure retaining Python runtime objects provides a traversal visitor implementation.

Accept when:
- All native structures holding Python runtime references implement visitor traversal methods returning pyo3::PyTraverseError on failure.
- Automated cyclic garbage collection and reference cycle test suites pass without memory leaks.
- The resolved pyo3 library version is validated against the repository lock artifact.

## Enforcement

- Verified by: Continuous integration test runs executing cyclic garbage collection test suites.
- Verified by: Code review verification of all native structs holding Python runtime references.
- Verified by: Static analysis and compiler checks verifying implementation of traversal visitor traits.
- Violation handling: Pull requests that introduce native structures holding Python references without pyo3 traversal implementations will be rejected.
- Violation handling: Test failures indicating cyclic reference memory leaks block merging into the main branch.
- Exception process: Exceptions require documented architectural justification demonstrating that the affected native type cannot participate in cyclic references.
- Exception process: Exception requests must be submitted and approved by core maintainers before implementation.