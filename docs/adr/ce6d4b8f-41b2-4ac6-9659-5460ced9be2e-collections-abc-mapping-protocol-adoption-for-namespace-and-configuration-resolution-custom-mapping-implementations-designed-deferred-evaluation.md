# collections.abc Mapping Protocol Adoption for Namespace and Configuration Resolution: Custom Mapping Implementations Designed Deferred Evaluation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Runtime type evaluation routines require mapping structures for namespace resolution during annotation inspection.
- Aggregating multiple lexical scopes eagerly causes redundant memory allocation and computation when only a subset of keys are referenced.
- Standard library evaluation mechanisms expect objects conforming to mapping protocols while type checkers enforce formal interface satisfaction.

## Problem Statement

Evaluating type annotations across multiple layered scopes requires passing mapping structures to evaluation runtimes. Merging global and local namespaces eagerly upon context initialization introduces unnecessary computational overhead and memory allocation when lookups are sparse, while ad-hoc dictionary-like objects fail formal type checking against standard library mapping contracts.

## Decision

1. MUST: Custom mapping implementations designed for deferred evaluation MUST defer multi-scope dictionary merging until attribute or item retrieval occurs.

## Policy Block

- MUST Custom mapping implementations designed for deferred evaluation MUST defer multi-scope dictionary merging until attribute or item retrieval occurs.

In scope:
- All internal modules defining custom mapping objects for evaluation namespaces.
- Configuration processing routines accepting or merging dictionary-like settings.

Out of scope:
- External user-facing dictionary models not involved in namespace or configuration resolution.
- Primitive scalar configuration fields and non-mapping metadata attributes.

Exceptions:
- EXC-20-001: Direct dictionary instances are supplied as self-contained namespaces without layered scoping requirements.

## Rationale

- Subclassing collections.abc.Mapping satisfies static type checkers and runtime evaluation functions while permitting lazy evaluation patterns.
- Encapsulating scope hierarchies within lazy mapping wrappers preserves namespace precedence rules without mutating parent scopes.
- Using cached properties within custom mapping structures balances lazy initialization with efficient repeated lookups.

## Consequences

Positive:
- Defers expensive multi-scope dictionary merging until a lookup is actually performed during type annotation evaluation.
- Ensures complete static type safety and contract compatibility with standard library evaluation routines expecting mapping interfaces.
- Preserves priority precedence across layered namespace boundaries without mutating underlying scope mappings.

Negative:
- Custom mapping implementations must implement all abstract methods of the collection contract to satisfy type checkers even if only item retrieval is exercised at runtime.
- Caching properties across multiple namespaces introduces memory retention overhead for the lifetime of the mapping instance once accessed.

## Alternatives

- Eager dictionary aggregation of parent and local namespaces upon initialization (rejected)
  Rejected because: Eager merging incurs immediate performance penalties and memory allocations for namespaces that may only require sparse key lookups during annotation evaluation.
  When valid: Valid when namespace scopes are static, small, and guaranteed to be fully accessed during every evaluation pass.
- Unconstrained custom mapping objects implementing only magic item retrieval methods without formal abstract base class inheritance (rejected)
  Rejected because: Failing to inherit from the formal collections.abc mapping abstract class breaks static type analysis and causes runtime incompatibilities when interoperating with standard mapping consumers.
  When valid: Valid in minimal execution contexts where static type validation is entirely bypassed and only basic indexing operations are required.

## Risks

- Stale cached namespace data if underlying scopes mutate after the cached property has materialized.
  Mitigation: Ensure cached properties are read-only and underlying namespace sequences remain immutable after construction.
  Owner: Core Engineering Team
- Partial implementation of abstract mapping methods causing unexpected runtime errors when consumed by generic mapping utilities.
  Mitigation: Implement comprehensive test suites covering length, iteration, and membership behavior alongside standard key access.
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
- Implementations of lazy mappings should encapsulate underlying scope sequences in private attributes and expose merged views through cached property routines.
- Mapping protocols must fulfill length, iteration, and membership checks in terms of the underlying cached representation.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking suite to confirm all custom mapping classes satisfy the collections.abc.Mapping protocol.
- Discover and execute the repository test runner targeting namespace resolution and configuration processing modules.

Accept when:
- All test suites exercising namespace evaluation and configuration resolution pass without protocol mismatch errors.
- Static type checking passes across all custom collections.abc mapping implementations without protocol omissions.
- Namespace construction benchmarks show deferred evaluation without eager dictionary materialization upon initialization.

## Enforcement

- Verified by: Automated static type analysis in the continuous integration pipeline verifying interface adherence.
- Verified by: Unit test suites validating dictionary contract compliance and priority resolution ordering.
- Verified by: Code review verification for any new namespace or configuration data structures.
- Violation handling: Build and test failures triggered by static type checker errors on incomplete mapping protocols.
- Violation handling: Rejection of pull requests introducing eager dictionary merges in annotation evaluation paths.
- Exception process: Exceptions require architectural review documenting why a custom mapping implementation cannot inherit from the standard abstract base class.
- Exception process: Any deviation from deferred resolution must provide benchmark evidence demonstrating zero performance regression.