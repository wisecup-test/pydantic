# Thread-Safe Lazy Initialization of Python Runtime Objects via pyo3::sync::PyOnceLock: Cached Values Stored Pyo3 Sync Pyoncelock

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Interaction between native compiled extension code and an external runtime requires dynamic type resolution, exception constructor discovery, and singleton object acquisition.
- Performing repeated module lookups across the foreign function interface within hot validation and serialization paths introduces substantial execution overhead.
- Concurrent multi-threaded execution environments require thread-safe static synchronization to guarantee that shared runtime references are initialized exactly once without race conditions.
- The native codebase uniformly adopts pyo3::sync::PyOnceLock across validation, serialization, input handling, and error definition modules to cache runtime types and singletons.

## Problem Statement

Native extension routines frequently interact with foreign runtime classes, error definitions, and singleton objects during validation and serialization. Resolving these objects dynamically on every invocation introduces cross-language call overhead, while naive unsynchronized static caching risks data races across concurrent threads. A standardized, thread-safe lazy initialization primitive is required to safely store and retrieve shared foreign runtime objects with minimal runtime penalty.

## Decision

1. MUST_NOT: Cached values stored in pyo3::sync::PyOnceLock MUST NOT retain mutable state that could introduce data races across concurrent threads accessing the shared reference.

## Policy Block

- MUST_NOT Cached values stored in pyo3::sync::PyOnceLock MUST NOT retain mutable state that could introduce data races across concurrent threads accessing the shared reference.

In scope:
- Native extension modules that require repeated access to foreign runtime types, exception classes, singletons, or lookup structures across threads.
- Validation, serialization, input handling, and error formatting routines interacting with the foreign language runtime.

Out of scope:
- Pure native data structures and algorithms operating without references to foreign runtime objects.
- Local or ephemeral instances where single-thread ownership is maintained and no cross-thread caching is required.

## Rationale

- Evidence across thirteen distinct native modules shows consistent adoption of pyo3::sync::PyOnceLock for caching runtime types, error constructors, and sentinel values.
- pyo3::sync::PyOnceLock provides lock-free read access after initial resolution, minimizing cross-boundary foreign function interface overhead in hot execution paths.
- Standardizing on pyo3::sync::PyOnceLock ensures uniform synchronization semantics and avoids ad-hoc or unsafe static initialization across native components.

## Consequences

Positive:
- Eliminates repetitive dynamic imports and attribute lookups across the foreign runtime boundary in performance-critical paths.
- Guarantees thread-safe lazy initialization of shared runtime types and singletons without manual locking boilerplate.
- Standardizes static caching patterns across validators, serializers, error formatters, and input parsers.

Negative:
- First-access execution paths incur initialization overhead when instantiating the cached runtime objects.
- Increases process-lifetime static memory residency for cached runtime references and type metadata.
- Requires strict initialization closure scoping to prevent re-entrancy and deadlocks during interpreter acquisition.

## Alternatives

- Dynamic per-call resolution and import of foreign runtime types (rejected)
  Rejected because: Incurs significant cross-boundary lookup overhead and repetitive interpreter synchronization on hot execution paths.
  When valid: When target types or modules are dynamically configured and cannot be determined at initialization time.
- Standard library once-lock synchronization primitives without foreign runtime integration (rejected)
  Rejected because: Fails to coordinate with foreign runtime lifecycle and interpreter thread state management during static initialization.
  When valid: When caching purely native structures that hold no references to foreign runtime objects.

## Risks

- Potential initialization deadlocks if initialization closures trigger re-entrant access to the same lock.
  Mitigation: Keep initialization closures minimal, self-contained, and free of circular or nested type acquisitions.
  Owner: engineering team
- Uncontrolled memory retention across long-running application lifecycles from immortal static references.
  Mitigation: Restrict pyo3::sync::PyOnceLock caching to immutable type definitions, singletons, and lookup structures.
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
- Encapsulate static pyo3::sync::PyOnceLock instances behind accessor functions that return borrowed references to the cached runtime object.
- Keep initialization closures minimal and self-contained to avoid triggering nested calls that could cause deadlock.

## Continuation Context


Verify commands:
- Discover the workspace configuration manifest and execute the test runner to validate synchronization invariants.
- Inspect native source files to confirm that static foreign runtime objects use pyo3::sync::PyOnceLock instead of unsynchronized static state.

Accept when:
- All native test suites execute cleanly with zero thread contention errors or deadlocks during concurrent execution.
- Static foreign runtime object access across core modules is mediated by pyo3::sync::PyOnceLock initialization routines.

## Enforcement

- Verified by: Continuous integration verification suites testing native extension execution across concurrent threads.
- Verified by: Peer code review of changes to native extension modules.
- Violation handling: Pull requests introducing unsynchronized static state or repeated dynamic type lookups in hot paths are rejected until updated to use pyo3::sync::PyOnceLock.
- Exception process: Exceptions require review and approval by native runtime maintainers with accompanying benchmark results demonstrating why static synchronization is not viable.