# weakref Module Adoption for Metaclass Namespace Caching and Reference Management: Subclass Registration Namespace Interception Hooks That

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Metaclass execution captures caller frame namespaces to resolve forward references, type annotations, and generic parameters during class construction.
- Direct retention of parent frame namespaces introduces strong reference cycles between class objects, namespace dictionaries, and caller scopes, preventing timely garbage collection.
- The standard library weakref module provides weak reference primitives and weak dictionary structures that decouple metadata inspection from object lifetime management.
- Consistent reference isolation across internal utilities and class construction metaclasses prevents memory accumulation in long-running processes that dynamically generate models.

## Problem Statement

During dynamic class construction, metaclasses must access parent frame namespaces to inspect type annotations, generic parameters, and configuration blocks. Retaining strong references to these namespaces binds active execution frames and module globals to the newly created class object, creating circular references that hinder garbage collection and cause memory leaks in long-running applications. The framework requires a standardized reference management strategy to access parent scope metadata while permitting unreferenced frame objects to be reclaimed immediately.

## Decision

1. MUST: Subclass registration and namespace interception hooks that monitor decorator overrides or attribute assignments MUST maintain decoupled reference graphs that do not block garbage collection of target descriptors.

## Policy Block

- MUST Subclass registration and namespace interception hooks that monitor decorator overrides or attribute assignments MUST maintain decoupled reference graphs that do not block garbage collection of target descriptors.

In scope:
- Metaclass construction routines capturing parent scopes and caller frame namespaces.
- Internal reflection utilities and dictionary wrappers caching type resolution metadata.

Out of scope:
- Static class attributes containing primitive immutable constants.
- Model fields where values are copied by value rather than referenced across scopes.

## Rationale

- Evidence across internal model construction and utility modules demonstrates reliance on weakref primitives to prevent memory leaks during dynamic type generation.
- Wrapping parent namespaces in weak-value dictionary structures ensures forward reference resolution succeeds while allowing transient execution frames to be collected.
- Standardizing on weakref eliminates divergent object lifecycle caching strategies across core reflection helpers.

## Consequences

Positive:
- Prevents circular references between generated model classes and parent execution frames, eliminating memory leaks in long-running applications.
- Enables accurate forward reference and annotation resolution without artificially pinning caller scopes.
- Provides consistent, memory-safe namespace handling across both modern and compatibility utility modules.

Negative:
- Requires defensive lookup patterns to unpack weak references that may become invalidated if the referenced target expires.
- Introduces minor runtime overhead during class construction to construct and unpack weak dictionary structures.

## Alternatives

- Direct Strong Reference Storage of Parent Frame Namespaces (rejected)
  Rejected because: Holding strong references to parent frame dictionaries causes severe memory retention issues by keeping entire stack frames and module namespaces alive indefinitely.
  When valid: Valid only in short-lived single-script executions where memory lifecycle is managed entirely by immediate process termination.
- Full Deep Copy of Parent Namespaces at Class Definition (rejected)
  Rejected because: Deep copying namespace dictionaries at class definition is computationally expensive and fails when namespaces contain uncopyable objects such as active modules or socket descriptors.
  When valid: Valid only when namespaces contain exclusively primitive immutable types.

## Risks

- Premature garbage collection of parent namespaces before type evaluation completes could cause unresolved forward reference errors.
  Mitigation: Retain module-level scope references and implement lenient fallback resolution when weak references expire.
  Owner: Core Framework Engineering Team
- Inconsistent weak reference unpacking across internal utilities could lead to key lookup failures.
  Mitigation: Standardize access through centralized namespace unpacking helpers with comprehensive test suites.
  Owner: Core Framework Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct weak reference dictionaries using specialized helpers that gracefully tolerate un-weakreferenceable objects by ignoring or handling them leniently.
- Ensure all namespace resolver components unpack weak-value mappings immediately prior to attribute inspection to minimize stale reference windows.

## Continuation Context


Verify commands:
- Discover and run the project static analysis suite to verify weakref usage across metaclass construction and utility modules.
- Discover and execute the project test runner targeting model construction and namespace resolution test suites to verify garbage collection behavior.

Accept when:
- All static analysis checks pass with zero reported violations regarding reference cycles or improper namespace caching.
- Automated test suites pass, verifying that dynamic model creation releases parent frame references without memory leakage.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline.
- Verified by: Peer code review mandatory for all changes touching metaclass construction and reference handling.
- Verified by: Automated memory regression test suites monitoring object retention after model generation.
- Violation handling: Pull requests introducing strong references to parent frame namespaces will fail automated reviews and be blocked from merging.
- Violation handling: Violations identified in post-merge audits must be remediated immediately with weak reference wrappers.
- Exception process: Exceptions require documented justification proving that weak referencing is technically impossible and must be approved by the core framework architecture lead.