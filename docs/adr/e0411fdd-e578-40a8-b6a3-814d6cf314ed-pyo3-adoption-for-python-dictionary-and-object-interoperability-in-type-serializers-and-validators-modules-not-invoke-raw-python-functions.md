# PyO3 Adoption for Python Dictionary and Object Interoperability in Type Serializers and Validators: Modules Not Invoke Raw Python Functions

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Native serialization and validation routines require direct interaction with Python dictionaries and object representations at the runtime boundary.
- Direct CPython FFI usage without typed wrappers introduces unsafe memory management overhead and maintenance complexity.
- The codebase standardizes on the PyO3 library to provide safe, idiomatic bindings, type wrappers including pyo3::types::PyDict, and string interning mechanisms.

## Problem Statement

Serializing native structures to Python objects and validating Python inputs in a high-throughput runtime requires safe, low-overhead interactions with Python dictionaries and strings. Without a unified FFI abstraction layer, modules risk unsafe pointer arithmetic, duplicate string allocations, and inconsistent type conversion across serializers.

## Decision

1. MUST_NOT: Modules MUST_NOT invoke raw Python C API functions directly when equivalent high-level PyO3 abstractions exist in the resolved PyO3 crate.

## Policy Block

- MUST_NOT Modules MUST_NOT invoke raw Python C API functions directly when equivalent high-level PyO3 abstractions exist in the resolved PyO3 crate.

In scope:
- Native type serializers and validator components interfacing with Python runtime objects.
- Modules constructing or traversing Python dictionary structures across language boundaries.

Out of scope:
- Pure internal data processing pipelines that operate solely on native Rust data structures without Python runtime interaction.
- Build-time code generation scripts that do not run inside the active Python interpreter process.

Exceptions:
- EXC-20-001: A specialized serializer requires direct C-level SIMD or custom buffer protocol access where PyO3 abstractions impose measurable latency.

## Rationale

- Adopting PyO3 provides memory-safe abstractions over CPython FFI, reducing unsafe blocks and preventing memory corruption during dictionary traversal.
- Standardizing on pyo3::types::PyDict and pyo3::intern minimizes Python heap allocations during repeated dictionary serialization passes.
- Evidence across nine serializer and validator modules demonstrates that uniform PyO3 trait implementations ensure consistent error propagation and type conversion behavior.

## Consequences

Positive:
- Consistent and type-safe Python dictionary manipulation across all type serializers.
- Reduced runtime memory allocations through shared string interning and copy-on-write references.
- Elimination of ad-hoc unsafe CPython FFI calls across validation and serialization pipelines.

Negative:
- Tight coupling of core serializer internals to the PyO3 API surface and release lifecycle.
- Thread synchronization constraints imposed by Python Global Interpreter Lock semantics during PyO3 object interaction.

## Alternatives

- Direct manual CPython C API bindings without PyO3 wrappers (rejected)
  Rejected because: Manual FFI bindings require extensive unsafe code blocks and significantly increase the risk of reference counting errors and memory leaks.
  When valid: When building minimal standalone binary extensions where dependency footprint must remain near zero.
- Intermediate JSON serialization buffer bridging Rust and Python (rejected)
  Rejected because: Intermediate serialization introduces significant CPU and allocation overhead compared to direct in-memory Python object construction.
  When valid: When crossing process boundaries or remote RPC interfaces rather than in-process FFI.

## Risks

- Breaking API changes across upstream PyO3 library versions could impact multiple serializer implementations simultaneously.
  Mitigation: Pin exact dependency versions in repository lock artifacts and encapsulate PyO3 type interactions behind standardized trait boundaries.
  Owner: Core Engine Team
- Incorrect thread access to PyO3 objects outside interpreter acquisition can cause runtime panics.
  Mitigation: Enforce thread-safety boundaries using standard synchronization primitives and copy-on-write wrappers.
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
- Verify that any new type serializer exposes uniform conversion traits conforming to existing PyO3 dictionary patterns.
- Utilize string interning for fixed schema field names and dictionary keys to maximize performance in hot paths.

## Continuation Context


Verify commands:
- Discover and execute the repository unit and integration test suite targeting serialization and validation modules.
- Discover and execute the repository static analysis and linting verification suite to validate PyO3 type binding compliance.

Accept when:
- All unit and integration tests across type serializers and validators pass without errors.
- Code analysis passes with zero warnings related to PyO3 type conversions, reference counting, or FFI safety.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and lint suites.
- Verified by: Peer code review verifying adherence to PyO3 abstraction rules and absence of raw FFI calls.
- Violation handling: Pull requests introducing raw FFI calls or bypassing PyO3 abstractions will be blocked until refactored.
- Violation handling: Compiler or linter warnings regarding PyO3 safety invariants will trigger build failure.
- Exception process: Submit an architectural exception request detailing performance profiling data demonstrating necessity of raw FFI.
- Exception process: Require sign-off from two core maintainers and comprehensive safety documentation.