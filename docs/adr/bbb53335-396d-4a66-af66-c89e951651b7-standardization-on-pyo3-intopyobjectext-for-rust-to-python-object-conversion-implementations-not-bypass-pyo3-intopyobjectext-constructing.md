# Standardization on pyo3::IntoPyObjectExt for Rust-to-Python Object Conversion: Implementations Not Bypass Pyo3 Intopyobjectext Constructing

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Context

- The core data validation and serialization engine coordinates between compiled native data structures and interpreted foreign runtime environments.
- Transferring data across the foreign function interface boundary requires strict management of memory ownership, reference counts, and lifetime parameters.
- Analysis of eleven core modules across validators, serializers, and input parsing reveals widespread adoption of pyo3::IntoPyObjectExt for converting native structures into foreign objects.

## Problem Statement

Converting native data structures into foreign runtime objects without a unified conversion contract produces fragmented implementation patterns, increases the risk of memory leaks or lifetime errors, and creates inconsistent object allocation behavior across validators and serializers.

## Decision

1. MUST_NOT: Implementations MUST NOT bypass pyo3::IntoPyObjectExt by constructing unmanaged raw foreign pointers or invoking deprecated conversion traits for values crossing into the foreign runtime.

## Policy Block

- MUST_NOT Implementations MUST NOT bypass pyo3::IntoPyObjectExt by constructing unmanaged raw foreign pointers or invoking deprecated conversion traits for values crossing into the foreign runtime.

In scope:
- Native core validator routines returning validated structures to the foreign interpreter.
- Serializer definitions converting native structures into foreign objects or sequences.
- Input conversion and lookup key modules interfacing with foreign mappings and dictionary objects.

Out of scope:
- Internal pure native algorithms and data structures that do not cross the foreign function interface boundary.
- Low-level parser routines operating strictly within native memory buffers.

## Rationale

- Observation of pyo3::IntoPyObjectExt across eleven core files establishes that conversion across the foreign function interface boundary is standardized through a single trait extension rather than ad-hoc pointer manipulation.
- Unifying object conversion through the foreign function interface trait system enforces strict memory ownership and lifetime validation across the boundary between compiled native code and interpreted foreign runtime objects.
- Coordinating pyo3::IntoPyObjectExt with std::sync::Arc and std::borrow::Cow preserves zero-copy semantics where possible while providing thread-safe reference sharing for validator instances.

## Consequences

Positive:
- Eliminates disparate object conversion patterns by establishing a unified foreign function interface conversion contract across all validators and serializers.
- Enforces lifetime and ownership guarantees at compile time when transferring data ownership between native code and the foreign runtime.
- Reduces memory allocations by coupling standardized conversion traits with string interning and reference counting.

Negative:
- Couples internal serialization and validation logic to the specific API contracts of the underlying foreign function interface crate.
- Increases compile-time macro expansion overhead across validator and serializer codebases.
- Requires engine developers to master both native ownership semantics and foreign runtime memory models when writing conversion routines.

## Alternatives

- Manual raw foreign function pointer conversion and manual reference count increments (rejected)
  Rejected because: Manual raw pointer manipulation introduces severe risks of memory leaks, premature deallocations, and undefined behavior across the foreign boundary.
  When valid: Only when interacting with bare foreign ABIs lacking higher-level language bindings.
- Legacy untyped conversion routines without explicit trait-level ownership guarantees (rejected)
  Rejected because: Legacy conversion methods lack modern lifetime tracking and fail to provide compile-time verification of object safety.
  When valid: In legacy environments where modern foreign function interface trait extensions are unavailable.

## Risks

- Upstream changes to trait contracts in the foreign function interface library could require broad refactoring across multiple validators and serializers.
  Mitigation: Lock exact dependency versions using repository resolution artifacts and isolate trait conversions behind standard engine contracts.
  Owner: Core Engineering Team
- Accidental deep copying of large structures during conversion could introduce runtime latency spikes.
  Mitigation: Enforce the use of clone-on-write pointers and string interning during review and automated benchmark verification.
  Owner: Performance Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Inspect the project dependency declaration to identify the active foreign function interface library and verify that pyo3::IntoPyObjectExt is properly brought into scope within all conversion modules.
- When constructing complex collections such as mappings or sequences, assemble internal items before invoking conversion routines to minimize runtime interpreter lock contention.

## Continuation Context


Verify commands:
- Discover the workspace verification script in the repository configuration and execute the static analysis suite to confirm foreign function interface conversion compliance.
- Run the project test suite across all validator and serializer test fixtures to ensure converted foreign runtime objects match expected schemas and values.

Accept when:
- All validator and serializer conversion modules compile without trait mismatch errors or deprecation warnings.
- Test suites verifying foreign function interface object conversion pass with zero regressions in memory safety or conversion accuracy.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace compilation and test suites.
- Verified by: Mandatory peer code reviews validating that new validators and serializers implement pyo3::IntoPyObjectExt.
- Violation handling: Pull requests introducing raw foreign pointer manipulation or unapproved conversion mechanisms are automatically blocked by continuous integration.
- Violation handling: Code failing conversion standards must be refactored to use the canonical trait extension before merge approval.
- Exception process: Architectural exceptions require formal submission and unanimous sign-off by the core systems architecture team, with documented rationale for why canonical trait conversion cannot be used.