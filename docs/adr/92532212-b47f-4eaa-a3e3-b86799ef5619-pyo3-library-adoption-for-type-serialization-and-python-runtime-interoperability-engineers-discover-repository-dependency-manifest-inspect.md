# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Engineers Discover Repository Dependency Manifest Inspect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Native type serializers and validators interface continuously with the Python runtime to inspect, transform, and emit complex object structures.
- Previous disparate approaches to object instantiation and string handling risked performance degradation and reference counting discrepancies across runtime boundaries.
- Direct integration with Python garbage collection cycles and interning mechanisms is required to avoid memory leaks and maximize serialization throughput across diverse data structures.

## Problem Statement

High-performance native serialization and validation require tight integration with the host interpreter runtime without compromising memory safety or incurring repetitive conversion overhead. Without a unified foreign function interface architecture, individual type serializers risk divergent string interning strategies, inconsistent Python dictionary construction, and uncoordinated garbage collection traversal that can trigger catastrophic memory leaks in cyclic data structures.

## Decision

1. MUST: Engineers MUST discover the repository dependency manifest and inspect the authoritative repository lock artifact to determine and verify the exact resolved version of the foreign function interface library prior to implementing or modifying runtime bindings.

## Policy Block

- MUST Engineers MUST discover the repository dependency manifest and inspect the authoritative repository lock artifact to determine and verify the exact resolved version of the foreign function interface library prior to implementing or modifying runtime bindings.

In scope:
- Native type serializers and validator components interfacing with the Python runtime.
- Modules responsible for serialization state management, dictionary creation, and string interning.
- Components maintaining references to runtime Python objects requiring garbage collector traversal.

Out of scope:
- Pure internal logic and data structures that operate without Python runtime dependencies.
- Standalone utility routines that do not exchange values with the foreign runtime.

Exceptions:
- EXC-20-001: Pure native benchmark harnesses or isolated unit tests operating independently of the runtime interpreter environment.

## Rationale

- Static intermediate representation analysis across twenty-two serializer and validator modules demonstrates pervasive adoption of pyo3 abstractions for dictionary handling, interning, and cycle traversal.
- PyO3 provides mature, type-safe bindings that abstract raw interpreter pointers while exposing fine-grained memory controls like pyo3::intern and pyo3::pybacked::PyBackedStr necessary for high-throughput serialization.
- Embedding native traversal hooks via pyo3::gc::PyVisit directly satisfies the host interpreter cyclic garbage collection protocol.

## Consequences

Positive:
- Standardizes Python runtime interop patterns across all type serializers, ensuring predictable memory safety and exception handling.
- Improves serialization throughput by utilizing interned identifiers via pyo3::intern and zero-copy string references via pyo3::pybacked::PyBackedStr.
- Guarantees safe memory reclamation for complex cyclic structures through pyo3::gc::PyVisit traversal integration.

Negative:
- Couples native serializer and validator architecture tightly to the foreign function interface library abstractions.
- Increases cognitive overhead for contributors who must understand safe reference handling and garbage collection traversal protocols.

## Alternatives

- Direct manual C-API bindings using raw pointer manipulation and manual reference count tracking (rejected)
  Rejected because: Manual reference counting and raw pointer manipulation introduce significant risk of memory leaks, use-after-free conditions, and unhandled exception states.
  When valid: Only valid in constrained scenarios where external library dependencies are strictly prohibited and minimal binary footprint is paramount.
- Complete abstraction behind a proprietary foreign function interface wrapper module (rejected)
  Rejected because: Adds redundant indirection layers over established safe abstractions without providing performance or ergonomic benefits.
  When valid: Valid when targeting multiple disparate script runtime engines simultaneously.

## Risks

- Failure to register runtime Python objects in garbage collection traversal causes cyclic memory leaks.
  Mitigation: Enforce pyo3::gc::PyVisit implementations and automated leak regression checks across all serializer types retaining runtime object handles.
  Owner: Core runtime engineering team
- Misuse of interned strings across thread boundaries or outside active runtime contexts leads to undefined behavior.
  Mitigation: Constrain pyo3::intern invocations to validated active interpreter token contexts verified through code review and automated linting.
  Owner: Architecture and core maintainers

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Verify that all new type serializers handling mapping objects construct outputs via pyo3::types::PyDict interfaces and leverage pyo3::intern for repeated field names.
- Ensure any custom serializer holding references to Python objects implements cycle traversal to satisfy garbage collection requirements.

## Continuation Context


Verify commands:
- eval "${DISCOVERED_TEST_COMMAND:?Please set to project discovered test command}"
- eval "${DISCOVERED_LINT_COMMAND:?Please set to project discovered lint command}"

Accept when:
- All native serializer and validator modules compile cleanly and pass full automated test suites with active interpreter bindings.
- Memory leak detection suites confirm zero cyclic memory retention across repeated serialization cycles with complex objects.
- Static analysis checks verify that all repetitive field identifier lookups utilize interned string abstractions.

## Enforcement

- Verified by: Automated continuous integration test suites executing memory leak and garbage collection traversal verifications.
- Verified by: Mandatory peer code review for any new or modified type serializers interfacing with runtime interpreter types.
- Verified by: Static linting and compiler checks enforcing correct trait implementations and lifetime bindings.
- Violation handling: Pull requests omitting required garbage collection traversal or misusing raw interpreter references are blocked from merging.
- Violation handling: Detected memory leaks in serialization benchmarks require immediate remediation before release qualification.
- Exception process: Submit an architectural review request detailing why foreign runtime abstraction bypass is required.
- Exception process: Obtain explicit sign-off from core maintainers and provide automated memory safety validation benchmarks.