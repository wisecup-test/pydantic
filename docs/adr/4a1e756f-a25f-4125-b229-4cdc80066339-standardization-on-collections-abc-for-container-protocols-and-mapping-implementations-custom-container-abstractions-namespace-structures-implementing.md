# Standardization on collections.abc for Container Protocols and Mapping Implementations: Custom Container Abstractions Namespace Structures Implementing

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Context

- Internal subsystems require custom mapping abstractions for lazy namespace evaluation, schema generation overrides, and mock validation handlers.
- Relying on ad-hoc dictionary interfaces or concrete dictionary subclasses causes inconsistencies across type checkers and runtime inspection routines.
- Standardizing container protocols on a shared abstract base class ensures uniform behavioral expectations across internal module boundaries.

## Problem Statement

Internal modules require specialized container behaviors, such as deferred evaluation of namespaces and mock schema wrappers, but using raw primitive mappings or ad-hoc classes leads to fragile duck typing, inconsistent runtime inspections, and type-checking failures across subsystem boundaries.

## Decision

1. MUST: All custom container abstractions and namespace structures implementing mapping semantics MUST inherit directly from collections.abc.Mapping and implement the full protocol contract.

## Policy Block

- MUST All custom container abstractions and namespace structures implementing mapping semantics MUST inherit directly from collections.abc.Mapping and implement the full protocol contract.

In scope:
- Internal library modules authoring custom namespace, dictionary, or sequence data structures.
- Subsystem boundaries defining structural container protocols and type contracts.

Out of scope:
- Concrete standard library data structures used directly without behavioral extension or lazy evaluation.
- External third-party integration boundaries where foreign APIs mandate invariant concrete data structures.

Exceptions:
- EXC-20-001: A foreign interface strictly requires a primitive dictionary structure and rejects abstract collections.abc.Mapping subclasses.

## Rationale

- Inheriting from collections.abc.Mapping establishes a unified structural contract that guarantees duck-typing interoperability across internal schema and evaluation pipelines.
- Standard abstract collection protocols allow lazy evaluation wrappers to defer computation of namespace entries until key access occurs without breaking mapping consumers.
- Using collections.abc rather than legacy module aliases ensures future-proof compatibility across supported language runtime versions.

## Consequences

Positive:
- Provides consistent container behavior across internal evaluation, mock schema, and namespace wrappers.
- Enables memory and performance optimizations such as lazy evaluation without breaking mapping protocol contracts.
- Ensures strict compatibility with static type checkers and runtime protocol inspection across internal boundaries.

Negative:
- Requires custom classes to implement all abstract methods of the container interface even if only a subset is used at runtime.
- Introduces slight object overhead compared to raw primitive dictionary structures.

## Alternatives

- Direct inheritance from primitive dictionary types (rejected)
  Rejected because: Subclassing primitive dictionary structures causes inconsistent behavior with internal lookup routines, bypasses protocol encapsulation, and prevents clean lazy evaluation.
  When valid: When raw performance demands strictly forbid Python-level attribute dispatch and encapsulation is unnecessary.
- Ad-hoc duck typing without abstract base class inheritance (rejected)
  Rejected because: Fails static type checker protocol validation and prevents runtime type inspection across module boundaries.
  When valid: When authoring ultra-minimal protocol wrappers where class hierarchy inheritance is explicitly prohibited.

## Risks

- Incomplete implementation of abstract methods causing runtime instantiation failures.
  Mitigation: Enforce static type checking and comprehensive instantiation tests during verification pipelines.
  Owner: Engineering Team
- Performance degradation if lazy property caching is misconfigured on container access.
  Mitigation: Profile hot paths and benchmark mapping access routines across schema evaluation workloads.
  Owner: Core Infrastructure Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When subclassing collections.abc.Mapping, developers must provide implementations for __getitem__, __len__, and __iter__. Mixin methods like get, keys, values, items, and __contains__ are inherited automatically but can be overridden if specialized performance characteristics are needed.
- Lazy mapping implementations should compute the underlying mapping data once upon demand and retain the resolved state to satisfy repeated key lookups without repeated overhead.

## Continuation Context


Verify commands:
- bash -c 'DISCOVERED_CHECKER=$(command -v "${PROJECT_TYPE_CHECKER:-}" || true); if [ -n "$DISCOVERED_CHECKER" ]; then "$DISCOVERED_CHECKER"; else echo "Discover and run the project type checking script from the repository manifest"; fi'
- bash -c 'DISCOVERED_TEST_RUNNER=$(command -v "${PROJECT_TEST_RUNNER:-}" || true); if [ -n "$DISCOVERED_TEST_RUNNER" ]; then "$DISCOVERED_TEST_RUNNER"; else echo "Discover and run the project test suite from the repository manifest"; fi'

Accept when:
- All custom container and namespace classes inherit from collections.abc abstract base classes and pass protocol validation tests.
- Verification suites confirm that static type checking and runtime instantiation succeed without abstract method errors.
- Lock artifact version grounding has been performed and verified against the repository configuration.

## Enforcement

- Verified by: Automated static analysis and type checking in continuous integration pipelines.
- Verified by: Peer code review verification against abstract container protocol rules.
- Violation handling: Pull requests introducing unapproved ad-hoc container structures or incomplete abstract base class implementations are blocked.
- Violation handling: Violations detected during review must be refactored to inherit from collections.abc.Mapping before merge.
- Exception process: Exceptions require written architectural justification submitted to core maintainers detailing why standard collection protocols cannot be satisfied.
- Exception process: Approved exceptions must be scoped strictly to designated boundary adapters.