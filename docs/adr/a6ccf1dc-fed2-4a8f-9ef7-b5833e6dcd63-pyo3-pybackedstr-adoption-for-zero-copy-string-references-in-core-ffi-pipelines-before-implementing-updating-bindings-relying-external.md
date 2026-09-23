# PyO3 PyBackedStr Adoption for Zero-Copy String References in Core FFI Pipelines: Before Implementing Updating Bindings Relying External

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- High-throughput serialization and validation pipelines process millions of string-keyed mappings, attribute lookups, and function argument records crossing the runtime language boundary.
- Allocating standard owned heap strings for each boundary interaction introduces significant memory allocation overhead and redundant string copying.
- Foreign function interface bindings require maintaining memory safety and reference validity while accessing string data without cloning byte buffers.

## Problem Statement

Repeated allocation and copying of string data across the foreign function interface boundary creates memory pressure and runtime latency during high-frequency validation and serialization workflows. The system requires an architectural mechanism to safely borrow and hold references to foreign runtime string buffers without eager heap reallocation while ensuring sound lifetime and reference management.

## Decision

1. MUST: Before implementing or updating bindings relying on external dependency APIs, consumers MUST discover the project dependency manifest and lock artifact to determine and verify the exact resolved version in the project documentation.

## Policy Block

- MUST Before implementing or updating bindings relying on external dependency APIs, consumers MUST discover the project dependency manifest and lock artifact to determine and verify the exact resolved version in the project documentation.

In scope:
- Components implementing key lookup, field extraction, argument parsing, or type serialization across the foreign function interface boundary.
- Internal data structures retaining string keys, field identifiers, or schema definition references derived from foreign runtime objects.

Out of scope:
- Internal pure algorithms operating entirely on static string literals or compile-time constants.
- Subsystems where string values must be modified in-place or detached completely from foreign runtime memory management.

Exceptions:
- EXC-20-001: A target string requires dynamic mutation or transformation that cannot be represented in a read-only foreign-backed string view.

## Rationale

- Static analysis across core pipeline modules reveals consistent utilization of pyo3::pybacked::PyBackedStr across key lookup, argument validation, and type serialization interfaces.
- Directly referencing foreign string buffers avoids millions of transient heap allocations during hot-path data parsing and serialization passes.
- Binding string lifetimes to the underlying foreign runtime object ensures memory safety across language boundaries without sacrificing execution throughput.

## Consequences

Positive:
- Substantially reduces heap allocation pressure and garbage generation during high-frequency validation and serialization operations.
- Preserves valid string access across the foreign function interface boundary without byte duplication.
- Standardizes string representation across argument parsers, key lookups, and type serializers.

Negative:
- Ties internal string representation lifetimes to foreign runtime object management.
- Increases cognitive complexity when dealing with shared ownership and copy-on-write semantics across concurrent boundaries.

## Alternatives

- Standard owned heap string conversion for all boundary crossings (rejected)
  Rejected because: Incurs severe runtime overhead from continuous allocation, copying, and deallocation of string buffers on every field and argument lookup.
  When valid: Valid only when complete decoupling from the foreign runtime memory manager is mandatory.
- Raw borrowed string slices with manual lifetime annotations (rejected)
  Rejected because: Restricts structural caching and shared ownership across decoupled pipeline stages due to strict lifetime propagation requirements.
  When valid: Valid when references do not escape the immediate stack frame of the boundary function.

## Risks

- Premature release or garbage collection of the underlying foreign runtime object while a backed string reference is in scope.
  Mitigation: Ensure reference count management and backing container invariants are enforced at creation boundaries.
  Owner: Core engineering team
- API drift between external binding versions affecting buffer representation or ownership semantics.
  Mitigation: Strict lockfile inspection and comprehensive cross-boundary regression testing.
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
- When constructing lookup keys and field mappings, convert foreign strings into pyo3::pybacked::PyBackedStr at the earliest boundary entry point to prevent intermediate allocations.
- Combine pyo3::pybacked::PyBackedStr with shared reference wrappers where serializers or validators need to share key definitions across sub-validators.

## Continuation Context


Verify commands:
- Discover and execute the project compilation test suite to verify foreign function interface type compatibility and lifetime constraints.
- Discover and run the project memory and performance benchmark suite to ensure absence of redundant string allocation regressions.
- Execute the project automated static analysis and linting verification routines to detect unintended conversions to owned heap strings.

Accept when:
- All compilation and test verification suites pass with zero lifetime errors or type mismatches across foreign function interface boundaries.
- Static analysis checks confirm that target string lookups and serialization fields utilize pyo3::pybacked::PyBackedStr without unexpected allocations.
- Performance benchmarks demonstrate the absence of allocation regressions in hot-path validation and serialization scenarios.

## Enforcement

- Verified by: Continuous integration static analysis pipelines
- Verified by: Peer code review for foreign function interface data paths
- Verified by: Automated benchmark regression suites
- Violation handling: Pull requests introducing eager string allocation on hot paths must be revised to use backed references.
- Violation handling: Static analysis warnings regarding boundary string handling block merge approval.
- Exception process: Exceptions must be documented in code with an architectural justification explaining why an owned heap string is required, followed by approval from module maintainers.