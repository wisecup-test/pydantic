# pyo3 Traversal and Conversion Protocol Integration for Domain Models: Domain Entities Not Bypass Garbage Collection

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Domain modeling across runtime boundaries requires structured synchronization between domain entities and host runtime environments.
- Domain representations such as serializers, key lookups, and validators interact with host language dictionary and string types requiring safe conversion and reference management.
- Cyclic references between domain structures and host objects necessitate active participation in garbage collection cycle traversal to prevent memory leaks.
- Standardizing domain modeling traits and traversal protocols using pyo3 ensures uniform lifecycle management and conversion across all domain components.

## Problem Statement

Domain entities, type serializers, and validators must interact with host runtime objects and reference cycles without inducing memory leaks or undefined state transitions. Without standardized traversal and conversion protocols, domain models risk inconsistent lifecycle management and untracked reference cycles across the domain boundary.

## Decision

1. MUST_NOT: Domain entities MUST_NOT bypass garbage collection traversal by storing unmanaged host object references in internal domain collections.

## Policy Block

- MUST_NOT Domain entities MUST_NOT bypass garbage collection traversal by storing unmanaged host object references in internal domain collections.

In scope:
- Domain model definitions, type serializers, schema validators, and lookup structures interacting with host runtime objects.
- Domain structures participating in cyclic references across the runtime boundary.

Out of scope:
- Pure internal utility functions with no host runtime object references or lifecycle dependencies.
- Self-contained value types that do not cross the domain boundary.

## Rationale

- Grounded domain models in pyo3 traversal protocols via PyVisit and PyTraverseError prevent reference cycle retention across boundary crossings.
- Standardizing object conversions via IntoPyObjectExt ensures consistent typing and error propagation across the domain modeling surface.
- Utilizing PyBackedStr and intern within domain key lookups reduces allocation pressure while preserving immutable string guarantees.

## Consequences

Positive:
- Ensures consistent lifecycle management and garbage collection visibility across all domain components.
- Provides structured error propagation and type-safe conversion via standardized pyo3 trait integration.
- Minimizes allocation overhead during repeated key lookups through specialized string representations.

Negative:
- Couples domain model definitions directly to pyo3 API contracts and runtime memory management semantics.
- Increases implementation boilerplate required to define explicit traversal visitors and conversion handlers across domain structures.

## Alternatives

- Manual Reference Counting and Raw Pointer Tracking (rejected)
  Rejected because: Manual pointer and reference management introduces significant risk of memory corruption, dangling pointers, and untracked reference cycles across the runtime boundary.
  When valid: When operating in standalone execution contexts without host language garbage collection interoperability.
- Full Domain Isolation via Serialized IPC (rejected)
  Rejected because: Serializing every domain boundary interaction across an inter-process barrier introduces prohibitive performance overhead for in-process execution.
  When valid: When domain boundaries cross separate process or network barriers.

## Risks

- Incomplete implementation of visitor traversal in complex domain types leading to undetected memory leaks.
  Mitigation: Enforce automated traversal verification and cycle detection tests on all domain models implementing boundary contracts.
  Owner: engineering team
- API drift across upstream pyo3 revisions affecting conversion and traversal signatures.
  Mitigation: Mandate lock-version verification and dependency pinning before adopting upstream changes.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Domain models requiring garbage collection integration must implement PyGcTraverse to coordinate with PyVisit callbacks and return PyTraverseError upon traversal failure.
- Domain serializers and lookup structures should leverage PyBackedStr and intern to ensure zero-copy or interned string references across boundary lookups.

## Continuation Context


Verify commands:
- Discover and run the project test execution script from the repository configuration to execute domain modeling and boundary traversal tests.
- Discover and run the static analysis and linting script declared in repository metadata to verify traversal contract compliance across domain modules.

Accept when:
- All domain models crossing the runtime boundary implement required traversal protocols without reporting untracked reference leaks.
- All verification scripts pass without type conversion errors or memory safety violations.

## Enforcement

- Verified by: Automated test suites executing domain traversal and lifecycle verification scripts in continuous integration pipelines.
- Verified by: Mandatory architectural code reviews verifying adherence to pyo3 conversion and traversal contracts.
- Violation handling: Pull requests introducing unmanaged cross-boundary references or failing traversal checks are blocked from merging until corrected.
- Exception process: Exceptions require documented architectural approval demonstrating that the exempted domain construct contains no cyclic references and cannot leak memory across boundaries.