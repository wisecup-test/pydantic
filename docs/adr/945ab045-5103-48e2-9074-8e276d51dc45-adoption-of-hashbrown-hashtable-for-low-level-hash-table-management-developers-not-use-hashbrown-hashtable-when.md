# Adoption of hashbrown::HashTable for Low-Level Hash Table Management: Developers Not Use Hashbrown Hashtable When

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Core runtime components require specialized hash table data structures that support fine-grained memory layout and manual bucket inspection.
- Standard high-level map structures encapsulate hashing and entry traversal, preventing integration with external runtime garbage collection and manual memory management.
- The codebase adopts hashbrown::HashTable across internal utility and runtime modules to gain direct control over hashing strategies, entry manipulation, and iteration.

## Problem Statement

Standard hash map abstractions enforce rigid key-value associations and automatic bucket management that obscure raw entry pointers and impose memory overhead. In performance-critical runtime operations and garbage-collection traversal routines, modules need direct access to bucket allocation, manual hashing, and custom entry layouts without incurring secondary map overhead.

## Decision

1. MUST_NOT: Developers MUST NOT use hashbrown::HashTable when standard key-value associative semantics and default collision resolution are sufficient, reserving it strictly for structures requiring manual entry management or custom traversal.

## Policy Block

- MUST_NOT Developers MUST NOT use hashbrown::HashTable when standard key-value associative semantics and default collision resolution are sufficient, reserving it strictly for structures requiring manual entry management or custom traversal.

In scope:
- Internal data structures requiring manual hashing, custom entry representations, or specialized garbage collection traversal.
- Performance-sensitive core routines where high-level map wrappers introduce unacceptable allocation or indirection overhead.

Out of scope:
- High-level module interfaces and general key-value storage where standard associative map containers fulfill requirements.
- External-facing public contracts that expose associative data collections to consumers.

## Rationale

- hashbrown::HashTable provides a low-level, high-performance hash table primitive that allows explicit control over bucket allocation and entry hashing.
- Evidence across multiple core modules demonstrates a deliberate architectural choice to use hashbrown::HashTable where low-level memory efficiency and manual traversal are necessary.
- Direct bucket control simplifies integration with runtime garbage collection traversal, ensuring that every contained reference is visited during cycle detection.

## Consequences

Positive:
- Provides direct control over memory layout and bucket allocation, eliminating redundant hashing and wrapper overhead.
- Enables precise entry traversal required for external cycle-detection and garbage collection systems.
- Reduces memory footprint by avoiding mandatory key-value pair encapsulation when single-entry or custom-bucket storage is desired.

Negative:
- Increases implementation complexity by requiring manual management of hashing, capacity growth, and entry validation.
- Introduces higher risk of invariant violations if table resizing or bucket state transitions are improperly coordinated.

## Alternatives

- Standard Library Associative Map (rejected)
  Rejected because: Standard library map types abstract away bucket management and do not expose low-level table manipulation or custom entry layout needed for specialized runtime mechanics.
  When valid: Valid for general application code and high-level components where standard key-value semantics suffice.
- Custom In-House Hash Table Implementation (rejected)
  Rejected because: Building and maintaining a bespoke hash table increases maintenance burden and carries high risk of suboptimal collision handling compared to a mature raw table implementation.
  When valid: Valid only when hardware-specific vector instructions or non-standard memory arenas cannot be supported by existing table primitives.

## Risks

- Manual bucket handling or incorrect hash calculation leading to lookup failures or corrupted table state.
  Mitigation: Encapsulate table interactions inside focused abstraction layers accompanied by comprehensive unit testing for insertion, growth, and removal invariants.
  Owner: Core Engineering Team
- Upstream API evolution across dependency releases altering entry indexing or allocation contracts.
  Mitigation: Enforce strict lock-version inspection and documentation verification prior to modifying table interaction logic.
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
- Construct domain-specific wrappers around hashbrown::HashTable to encapsulate bucket lifecycle, resizing thresholds, and hash generation.
- Ensure that all table iteration paths correctly handle occupied and vacant bucket states during traversal operations.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute the test suite covering hash table data structures and runtime traversal.
- Run the project linting and formatting checks to verify adherence to codebase standards and type safety constraints.

Accept when:
- All unit and integration tests covering hash table operations, capacity growth, and reference traversal pass with zero failures.
- Static analysis and type checks report no errors or unresolved table invariants.

## Enforcement

- Verified by: Automated continuous integration test suites executed on all pull requests.
- Verified by: Mandatory peer code review for any changes introducing or modifying low-level table structures.
- Violation handling: Pull requests introducing unencapsulated low-level table operations or bypassing lock-file grounding are blocked from merging.
- Exception process: Exceptions require written architectural review and sign-off from the core runtime maintainers detailing why standard containers cannot be used.