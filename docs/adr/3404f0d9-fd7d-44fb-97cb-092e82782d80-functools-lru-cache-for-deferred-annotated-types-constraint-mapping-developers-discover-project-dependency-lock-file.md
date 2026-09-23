# functools.lru_cache for Deferred annotated_types Constraint Mapping: Developers Discover Project Dependency Lock File

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Module initialization latency is a critical performance factor for core validation libraries, requiring non-essential dependencies to avoid eager evaluation at import time.
- Eager root-level imports of annotated_types introduce overhead for downstream consumers who do not utilize annotated constraint types.
- Maintaining type-to-constraint mapping dictionaries in multiple locations creates synchronization risks, necessitating a single canonical definition.
- Repeated local imports and dictionary instantiations inside un-memoized functions introduce unnecessary allocation and lookup overhead during schema generation.

## Problem Statement

Eagerly loading third-party type dependencies during library initialization degrades import performance across consumer applications, yet deferring imports to function scope incurs redundant dictionary construction and import resolution overhead during execution unless cached.

## Decision

1. MUST: Developers MUST discover the project dependency lock file and verify that the exact resolved version of annotated_types is confirmed prior to implementing or modifying constraint mapping definitions.

## Policy Block

- MUST Developers MUST discover the project dependency lock file and verify that the exact resolved version of annotated_types is confirmed prior to implementing or modifying constraint mapping definitions.

In scope:
- Internal metadata handling functions defining mappings between annotated types and internal constraint definitions.
- Performance-critical internal modules requiring deferred loading of external dependencies.

Out of scope:
- Public API boundaries where external types are part of exported function signatures.
- Dynamic runtime caches that require eviction policies, persistent storage, or distributed invalidation.

Exceptions:
- EXC-28-001: A type dependency is strictly mandatory for root module evaluation and cannot be deferred.

## Rationale

- Applying functools.lru_cache to a local factory function resolves the conflict between fast module import times and single-source-of-truth metadata mapping.
- Localizing imports of annotated_types eliminates module evaluation latency for consumers who do not require annotated constraint metadata.
- Memoizing the function result guarantees that mapping dictionaries are allocated only on first use and reused across subsequent schema building cycles.

## Consequences

Positive:
- Eliminates eager import latency caused by loading annotated_types at module evaluation time.
- Guarantees single instantiation of the mapping dictionary across the lifetime of the process.
- Centralizes type-to-constraint mapping logic in one memoized internal function.

Negative:
- Shifts import resolution errors from module load time to first function execution time.
- Retains the cached dictionary in process memory indefinitely unless cleared.
- Introduces a layer of indirection when accessing metadata constraint mappings.

## Alternatives

- Module-level global dictionary with eager top-level import (rejected)
  Rejected because: Regresses library import latency by eagerly evaluating dependencies during initial module load
  When valid: When the external dependency is universally required by every consumer workflow
- Un-memoized local function importing and creating dictionaries on every invocation (rejected)
  Rejected because: Incurs repeated dictionary construction and namespace resolution overhead during high-throughput schema processing
  When valid: When the mapping output depends dynamically on runtime arguments that vary widely
- Dedicated external cache service or key-value datastore (rejected)
  Rejected because: Introduces prohibitive distributed infrastructure dependencies for internal in-process metadata lookups
  When valid: When cache entries must be shared across disparate distributed worker processes

## Risks

- Deferred import failures remain dormant until the first invocation of the mapping function.
  Mitigation: Ensure automated test suites execute code paths that invoke the memoized mapping function during build verification.
  Owner: Core Engineering Team
- Accidental mutation of the cached dictionary by callers could corrupt subsequent metadata lookups.
  Mitigation: Ensure consuming routines treat the returned dictionary as read-only or construct immutable views.
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
- Define the internal mapping function with functools.lru_cache and contain the local import of annotated_types within the function body.
- Verify that callers retrieve constraints through the memoized function rather than re-importing the target dependency directly.

## Continuation Context


Verify commands:
- Discover the project test suite runner and execute all automated tests covering metadata collection and constraint mapping.
- Discover the project import benchmark scripts and verify that module import time meets established performance budgets without eager dependency loading.

Accept when:
- The deferred mapping function executes successfully and returns the expected constraint mapping without top-level import side effects.
- Subsequent invocations of the mapping function return the memoized dictionary without re-evaluating the local import.
- Automated test suites pass without regression in validation or schema generation pipelines.

## Enforcement

- Verified by: Continuous integration test suite executions validating metadata constraint generation.
- Verified by: Automated import tracking and profiling checks that flag unapproved top-level imports.
- Verified by: Peer code review ensuring new metadata mappings adopt memoized factory functions.
- Violation handling: Pull requests introducing top-level imports of deferred dependencies are blocked during code review.
- Violation handling: Static analysis checks detecting non-memoized repeated local imports must be resolved prior to merging.
- Exception process: Submit an architectural review request with import profiling data demonstrating why deferral is not feasible, approved by core maintainers.