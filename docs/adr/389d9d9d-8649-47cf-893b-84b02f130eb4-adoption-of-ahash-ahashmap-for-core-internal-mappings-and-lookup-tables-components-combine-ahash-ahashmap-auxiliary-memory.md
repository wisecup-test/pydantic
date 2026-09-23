# Adoption of ahash::AHashMap for Core Internal Mappings and Lookup Tables: Components Combine Ahash Ahashmap Auxiliary Memory

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- In-memory data structures across validation, serialization, error mapping, and garbage collection traversal require frequent key-value indexing and fast membership testing.
- The standard library hash map defaults to a cryptographically secure hashing algorithm designed to resist algorithmic complexity attacks, which introduces hashing overhead on hot execution paths.
- Core runtime routines frequently construct, populate, and query associative maps during model validation, schema definition resolution, and field serialization.
- Adopting a specialized, hardware-accelerated hashing collection provides deterministic performance gains across high-frequency mapping operations.

## Problem Statement

Standard library hash collections incur non-trivial hashing latency due to cryptographic denial-of-service defense mechanisms, creating performance bottlenecks across intensive schema validation, serialization field construction, and lookup operations where keys originate from trusted internal schemas and types.

## Decision

1. MAY: Components MAY combine ahash::AHashMap with auxiliary memory-optimization wrappers or reference-counted containers when storing composite schema structures or managing object lifetime traversals.

## Policy Block

- MAY Components MAY combine ahash::AHashMap with auxiliary memory-optimization wrappers or reference-counted containers when storing composite schema structures or managing object lifetime traversals.

In scope:
- Internal key-value lookup trees and mapping structures within validation, serialization, definition caches, and runtime traversal modules.
- In-memory mapping operations operating on trusted internal schemas, identifiers, and field specifications.

Out of scope:
- External-facing hash structures exposed directly to untrusted adversary-controlled keys requiring cryptographic collision resistance.
- Collections requiring persistent, cross-machine, or cross-run hash determinism.

## Rationale

- Six distinct core files substantiate direct integration with ahash::AHashMap across definitions, error types, lookup trees, serializers, and garbage collection traversal.
- Hardware-accelerated non-cryptographic hashing minimizes CPU cycle overhead during repeated dictionary and schema lookups in critical validation and serialization loops.
- Standardizing on a single high-performance map collection avoids hashing algorithm fragmentation across interdependent internal components.

## Consequences

Positive:
- Reduces latency during field lookup, definition retrieval, and serialization dispatch.
- Establishes a uniform, high-performance associative collection convention across core subsystems.
- Employs hardware-assisted hash generation without requiring manual hashing algorithm configuration at each call site.

Negative:
- Introduces an external collection dependency into the core workspace.
- Relinquishes standard library SipHash algorithmic denial-of-service collision resistance in hot-path maps.
- Requires developers to manage dependency lock synchronization when collection interfaces evolve.

## Alternatives

- Standard library default hash map (rejected)
  Rejected because: Default SipHash hashing introduces unnecessary latency in hot-path validation and serialization loops.
  When valid: When hashing untrusted external inputs requiring strict defense against hash-flooding denial-of-service attacks.
- BTreeMap for sorted key-value collections (rejected)
  Rejected because: Logarithmic lookup and insertion complexity fails to match constant-time hash map performance requirements for schema field access.
  When valid: When ordered key iteration or range-based queries are mandatory.

## Risks

- Hash collision vulnerability if untrusted adversary inputs are inserted into non-cryptographic hash maps without boundary validation.
  Mitigation: Confine ahash::AHashMap usage to internal schema definitions, validated fields, and trusted runtime metadata.
  Owner: Core Engineering Team
- Upstream dependency breaking changes across major collection releases.
  Mitigation: Enforce strict dependency resolution verification via lock artifacts before adopting upgraded collection releases.
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
- Replace standard associative collection types with ahash::AHashMap across internal caching and lookup structures, verifying that referenced keys implement the required hashing traits.
- When traversing references during runtime garbage collection cycles or building field serialization hierarchies, pre-allocate map capacity where collection sizes are known in advance.

## Continuation Context


Verify commands:
- Locate and execute the repository compilation and test runner scripts to confirm that all modules using ahash::AHashMap compile without type or borrow check errors.
- Execute the repository benchmark suite to verify that lookup tree and serialization throughput metrics remain within acceptable performance thresholds.

Accept when:
- All unit and integration test suites pass cleanly across all affected modules.
- Repository static analysis and compilation checks succeed with zero type or collection incompatibility errors.

## Enforcement

- Verified by: Automated continuous integration build checks verifying compilation and test passage.
- Verified by: Peer code review auditing all new associative collection instantiations across core modules.
- Violation handling: Code reviews will reject pull requests introducing default standard library hash maps for internal lookup or caching hot paths.
- Violation handling: Continuous integration checks failing benchmark or compilation standards must be resolved prior to merging.
- Exception process: An architectural exception may be approved by core engineering maintainers if cryptographic collision resistance or sorted key ordering is explicitly demonstrated to be necessary for a given subsystem.