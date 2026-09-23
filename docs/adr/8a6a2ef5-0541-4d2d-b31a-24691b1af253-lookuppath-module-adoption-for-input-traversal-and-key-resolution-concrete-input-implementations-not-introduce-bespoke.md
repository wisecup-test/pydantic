# LookupPath Module Adoption for Input Traversal and Key Resolution: Concrete Input Implementations Not Introduce Bespoke

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Input processing layers must support diverse data representations while providing unified field and nested path access semantics.
- Direct string manipulation or divergent traversal logic across input implementations risks behavioral inconsistency during key navigation.
- The codebase employs the shared LookupPath module across input abstractions and concrete string input types to establish a consistent path resolution boundary.

## Problem Statement

Heterogeneous input representations require a shared mechanism to resolve nested keys and lookup paths without replicating path traversal logic or introducing disparate key lookup semantics across input formats.

## Decision

1. MUST_NOT: Concrete input implementations MUST NOT introduce bespoke path traversal parsing logic that circumvents the LookupPath interface.

## Policy Block

- MUST_NOT Concrete input implementations MUST NOT introduce bespoke path traversal parsing logic that circumvents the LookupPath interface.

In scope:
- Input abstraction definitions and trait contracts governing data ingestion.
- Concrete input representation modules responsible for key resolution and value extraction.

Out of scope:
- Terminal scalar validation routines that do not participate in key navigation or structured path traversal.
- Unstructured streaming payload consumers that bypass key-value mapping.

## Rationale

- Consolidating traversal contracts into LookupPath ensures that nested lookups behave identically regardless of the underlying input representation.
- Evidence across both abstract input contracts and concrete string input implementations confirms standard use of LookupPath for navigating key structures.
- Encapsulating path resolution inside a dedicated module simplifies future optimizations and key retrieval caching strategies.

## Consequences

Positive:
- Guarantees unified semantics and error handling for path navigation across all input representations.
- Prevents code duplication across concrete input types by consolidating key lookup mechanics into a single module contract.
- Decouples high-level input validation contracts from low-level container indexing mechanisms.

Negative:
- Introduces compile-time coupling between input processing abstractions and the LookupPath module contract.
- Requires all newly added input representations to conform strictly to the LookupPath resolution protocol.

## Alternatives

- Implement independent ad-hoc key traversal and string splitting within each concrete input type (rejected)
  Rejected because: Causes duplicate traversal implementations across input representations and introduces divergent edge-case handling for nested keys
  When valid: Valid only in isolated single-file prototypes that have no integration with shared input abstractions
- Expose raw string slices and rely on consumer-level manual path resolution (rejected)
  Rejected because: Shifts navigation complexity and structural validation burden to downstream consumers while leaking representation details
  When valid: Valid only for flat key-value stores with zero support for nested path expressions

## Risks

- Breaking changes to LookupPath contract signature propagate across all concrete input implementations.
  Mitigation: Maintain comprehensive regression tests across all input formats to verify LookupPath interface conformity.
  Owner: engineering team
- Indirection through LookupPath abstractions could introduce overhead in tight validation loops.
  Mitigation: Profile key navigation performance across diverse lookup path depths to avoid allocation overheads.
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
- Ensure concrete input types handle both scalar keys and composite paths through the unified LookupPath resolution interface.
- Preserve lookup failure semantics consistently so downstream validation layers receive standardized path missing signals.

## Continuation Context


Verify commands:
- Discover and execute the project test runner to validate input traversal test suites.
- Run the project static analysis and linting verification suite to ensure compliance with LookupPath interface contracts.

Accept when:
- All input processing modules route key and nested attribute resolution through LookupPath contracts.
- Project verification suites pass without regression in input lookup or validation behavior.

## Enforcement

- Verified by: Automated test suite execution and continuous integration verification checks.
- Verified by: Peer code reviews validating that new input types implement LookupPath traversal contracts.
- Violation handling: Build or verification failures on non-compliant input traversal implementations.
- Violation handling: Code review rejection for input handling routines that bypass LookupPath contracts.
- Exception process: Architectural deviations require review and documented approval demonstrating that LookupPath cannot satisfy the specialized input traversal requirements.