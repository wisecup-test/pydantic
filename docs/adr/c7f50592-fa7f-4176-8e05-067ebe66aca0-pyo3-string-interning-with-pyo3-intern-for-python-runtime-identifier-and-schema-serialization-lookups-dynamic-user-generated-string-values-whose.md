# PyO3 String Interning with pyo3::intern for Python Runtime Identifier and Schema Serialization Lookups: Dynamic User Generated String Values Whose

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- High-throughput foreign function interface interactions between Rust serializers and Python runtime objects frequently access recurring attribute names, dictionary keys, and sentinel identifiers.
- Instantiating dynamic Python string objects on each serialization pass introduces heap allocation overhead and increases garbage collection pressure across the interpreter boundary.
- The codebase establishes a unified pattern across serialization builders and core utilities to obtain static, interned Python string references.

## Problem Statement

Serializing structured data across the Rust-Python foreign function interface requires frequent access to constant attribute names, dictionary keys, and sentinel objects. Performing runtime string conversion and heap allocation on every data point creates significant foreign function interface friction and latency, requiring a centralized mechanism to intern static string identifiers.

## Decision

1. MAY: Dynamic or user-generated string values whose values cannot be known at compile time MAY bypass pyo3::intern and use dynamic string conversion routines.

## Policy Block

- MAY Dynamic or user-generated string values whose values cannot be known at compile time MAY bypass pyo3::intern and use dynamic string conversion routines.

In scope:
- Rust serialization builders and type serializers interacting with Python runtime attributes or dictionary keys
- Common runtime utility modules managing static sentinel tokens or prebuilt schema constants

Out of scope:
- Dynamic user payload strings whose keys and contents are non-static and unbounded
- Pure Rust data transformations that do not cross the Python foreign function interface boundary

Exceptions:
- EX-20-001: A target Python string is dynamically computed at runtime and cannot be represented as a static string literal

## Rationale

- Static string interning via pyo3::intern avoids redundant heap allocations inside the Python interpreter for repetitive serialization keys across thirteen core modules.
- Reusing interned Python string pointers accelerates key matching in Python dictionary and attribute operations by allowing pointer identity comparison.
- Centralizing string interning across type serializers maintains consistent memory semantics across all supported serialization targets.

## Consequences

Positive:
- Eliminates repetitive heap allocations for static string identifiers during serialization execution.
- Improves attribute and dictionary lookup throughput by enabling direct pointer comparisons in the Python runtime.
- Enforces a consistent pattern for handling Python string references across serialization builders and sentinel utilities.

Negative:
- Interned string references remain cached within the Python interpreter for the duration of the runtime process.
- Requires string literals to be known at compile time or structured as static string references.

## Alternatives

- Dynamic Python string creation on each foreign function interface call (rejected)
  Rejected because: Repeated heap allocation and reference counting across the foreign function interface causes substantial latency and memory overhead during high-volume serialization.
  When valid: Only when handling user-provided dynamic string values that cannot be known at compile time.
- Manual static reference storage using custom global mutexes or atomic pointers (rejected)
  Rejected because: Manual synchronization increases implementation complexity, increases risk of deadlocks with the Python global interpreter lock, and duplicates functionality provided natively by pyo3::intern.
  When valid: When custom thread-local caching semantics are required that diverge from standard interpreter interning.

## Risks

- Unbounded growth of interned strings if applied mistakenly to dynamic or user-generated string values.
  Mitigation: Restrict pyo3::intern strictly to static string literals and constant schema identifiers.
  Owner: Core engineering team
- Behavioral divergence across different foreign function interface library revisions.
  Mitigation: Enforce strict dependency lockfile grounding and verify intern macro semantics in target runtime documentation.
  Owner: Core engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all call sites invoking pyo3::intern pass static string slices corresponding to stable Python attribute and dictionary keys.
- Coordinate interning operations with Python interpreter thread state tokens to guarantee valid foreign function interface access.

## Continuation Context


Verify commands:
- Discover the workspace dependency manifest and execute the test runner to ensure serializer modules compile and pass all tests.
- Discover and run the project code linter and static analysis suite to verify compliance with string interning rules.
- Execute the benchmark suite through the project build tool to validate that serialization overhead remains within expected performance boundaries.

Accept when:
- All unit and integration tests across type serializers pass without compilation or runtime errors.
- Static analysis confirms constant string keys across serializer definitions utilize pyo3::intern.
- No redundant Python string allocations are introduced for constant schema identifiers.

## Enforcement

- Verified by: Automated continuous integration build checks and test execution.
- Verified by: Peer code review for all modifications in serialization and runtime interop modules.
- Violation handling: Pull requests introducing dynamic string conversions for known static keys are flagged for revision.
- Violation handling: Deviations without approved architectural exceptions must be updated to use pyo3::intern.
- Exception process: Submit an architectural review request documenting why a dynamic string allocation is required over static interning.