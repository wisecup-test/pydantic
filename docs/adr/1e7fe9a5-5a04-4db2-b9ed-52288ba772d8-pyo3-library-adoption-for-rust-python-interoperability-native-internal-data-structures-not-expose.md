# pyo3 Library Adoption for Rust-Python Interoperability: Native Internal Data Structures Not Expose

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- The native engine serves as the high-performance validation and parsing core for a higher-level runtime environment.
- Crossing the foreign-function interface boundary requires disciplined synchronization, efficient memory sharing, and standardized error translation.
- Adopting pyo3 provides structured abstractions for runtime object conversion, interned symbol caching, and thread-safe lazy state initialization.

## Problem Statement

Direct foreign-function interoperability between native code and an external runtime environment introduces risks of memory corruption, allocation latency from unmanaged string conversions, uncontrolled native panics crossing foreign boundaries, and unsynchronized shared global state. The native core requires a standardized foreign-function interface library to govern type marshaling, symbol interning, exception propagation, and thread-safe initialization.

## Decision

1. MUST_NOT: Native internal data structures MUST_NOT expose raw foreign runtime pointers directly outside the designated foreign-function interface boundary modules.

## Policy Block

- MUST_NOT Native internal data structures MUST_NOT expose raw foreign runtime pointers directly outside the designated foreign-function interface boundary modules.

In scope:
- Native data structures and validation modules interfacing directly with foreign runtime objects.
- Components implementing boundary type conversions, static string interning, or runtime exception translation.

Out of scope:
- Pure internal native utility routines and mathematical operations operating strictly within native memory boundaries without runtime interaction.

Exceptions:
- EXC-20-001: Native error conditions require custom domain-specific exception types mapped to distinct runtime error classes.

## Rationale

- Standardizing on pyo3 eliminates disparate, ad-hoc foreign-function interface bindings across disparate validation routines.
- Using pyo3::sync::PyOnceLock guarantees thread-safe one-time initialization of runtime state without introducing external locking overhead or data races.
- Employing pyo3::intern significantly reduces allocation and reference-counting overhead for frequently checked dictionary keys and attributes.
- Deriving IntoPyObject and mapping validation failures to pyo3::exceptions::PyValueError enforces consistent boundary error handling and object marshaling across the core.

## Consequences

Positive:
- Unified cross-language boundary management reduces foreign-function interface defects and prevents native panics from destabilizing the host runtime.
- Cached interned strings and synchronized lazy locks provide measurable throughput gains during intensive validation cycles.
- Explicit type derivation standardizes data transformation contracts across input and return processing modules.

Negative:
- Tight coupling to pyo3 interfaces necessitates strict synchronization with the resolved runtime dependency version.
- Foreign-function interface boundaries impose cognitive overhead when debugging cross-language stack traces.
- Type marshaling abstractions require adherence to runtime thread-state and reference-counting semantics.

## Alternatives

- Hand-rolled C-compatible foreign-function interface with raw runtime pointer management (rejected)
  Rejected because: Manual pointer manipulation and reference counting substantially increase the risk of memory leaks, undefined behavior, and cross-boundary panics.
  When valid: Valid only in environments where zero external dependency overhead is strictly mandated and binding scopes are trivial.
- Generic foreign-function wrapper without dedicated runtime exception and interning primitives (rejected)
  Rejected because: Generic wrappers lack first-class support for thread-safe lazy initialization, interned strings, and native derive macros for object serialization.
  When valid: Valid when targeting multiple disparate host language runtimes simultaneously through a single neutral interface.

## Risks

- Breaking interface changes or deprecations across major updates of the adopted foreign-function library.
  Mitigation: Strict lock-file version resolution and verification against authoritative release documentation prior to interface upgrades.
  Owner: Core engineering team
- Overhead from improper usage of foreign runtime lock acquisition or premature object materialization.
  Mitigation: Enforce review of boundary contracts and mandate lazy initialization with PyOnceLock and string interning with intern.
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
- Ensure that all runtime exception conversions translate directly into standard exception types at the boundary and that native panics are strictly caught or prevented.
- Utilize static thread-safe storage abstractions for any global interpreter references to avoid repeated runtime environment acquisition.

## Continuation Context


Verify commands:
- Discover the project build and test runner scripts from the repository configuration and execute the suite to verify compilation and contract conformance of all foreign-function interface modules.
- Discover the linting and formatting verification tasks defined in the workspace configuration and execute them to validate that all pyo3 derives and imports comply with architectural guidelines.

Accept when:
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

## Enforcement

- Verified by: Continuous integration automated test suites exercising boundary conversion contracts and exception propagation.
- Verified by: Peer code review mandatory for all changes modifying or introducing foreign-function interface bindings.
- Violation handling: Pull requests introducing raw unmanaged foreign pointers, unhandled native panics at the boundary, or unapproved FFI libraries will be rejected.
- Violation handling: Compilation or test failures in boundary conversion modules block integration immediately.
- Exception process: Submit an architectural deviation request detailing the technical constraint preventing the use of standard library primitives to the core engineering team for formal review.