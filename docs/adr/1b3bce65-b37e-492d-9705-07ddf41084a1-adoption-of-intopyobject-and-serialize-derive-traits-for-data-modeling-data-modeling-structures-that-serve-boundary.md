# Adoption of IntoPyObject and Serialize Derive Traits for Data Modeling: Data Modeling Structures That Serve Boundary

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Data modeling layers require consistent serialization and language runtime conversion capabilities across boundary interfaces.
- Type representations frequently declare automatic trait implementations to satisfy public API contracts and domain modeling requirements.
- Static inspection identified a single module declaring derive implementations for IntoPyObject and Serialize alongside core standard traits, indicating localized rather than repository-wide adoption.

## Problem Statement

Return type models across domain boundaries must provide predictable serialization and foreign-function conversion contracts without manual boilerplate, while avoiding fragmented trait derivation practices across individual modules.

## Decision

1. MUST: Data modeling structures that serve boundary return contracts MUST derive Serialize and IntoPyObject traits to ensure uniform cross-boundary representation.

## Policy Block

- MUST Data modeling structures that serve boundary return contracts MUST derive Serialize and IntoPyObject traits to ensure uniform cross-boundary representation.

In scope:
- Data modeling structures defining return values across language boundaries
- Domain models requiring automated serialization and language conversion

Out of scope:
- Internal utility data structures that are not exposed across public API contracts
- Transient local variables not requiring persistent serialization

Exceptions:
- EXC-29-001: A data structure requires custom serialization behavior that standard derive macros cannot generate

## Rationale

- Deriving Serialize and IntoPyObject attributes standardizes data conversion pipelines across public interfaces, eliminating divergent manual marshaling code.
- Single-file static evidence indicates that while the pattern is technically sound, repository-wide architectural consensus has not yet been established across all domain modules.
- Automating trait derivation through declarative macros ensures compiler-checked correctness for return contract transformations.

## Consequences

Positive:
- Consistent cross-boundary data conversion and serialization behavior across domain return types.
- Reduced boilerplate and eliminated manual serialization logic for boundary data models.
- Compile-time enforcement of serialization and language conversion requirements.

Negative:
- Increased compile times due to macro expansion overhead on data structures.
- Coupling of data modeling representations to external trait contract dependencies.

## Alternatives

- Manual trait implementation for serialization and foreign language conversion (rejected)
  Rejected because: Manual trait implementations introduce repetitive boilerplate and increase the risk of inconsistent serialization logic across types
  When valid: Valid when custom serialization schemas deviate fundamentally from structural fields
- Ad-hoc conversion functions without declarative derive attributes (rejected)
  Rejected because: Ad-hoc conversion lacks uniform interface adherence and prevents generic trait bounds from operating on return types
  When valid: Valid for one-off conversions between disparate internal formats

## Risks

- Upstream dependency breaking changes in derivation macros could disrupt data model builds
  Mitigation: Verify exact resolved dependency versions in the repository lock file prior to dependency upgrades
  Owner: Core engineering team
- Macro expansion bloat impacting compilation performance
  Mitigation: Limit derive attributes to public boundary structures requiring explicit conversion capabilities
  Owner: Core engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Examine data modeling definitions in the core input subsystem to verify that all boundary types declare necessary derivation traits.
- Ensure public constructors align with strict and lax contract validation requirements across public boundary interfaces.

## Continuation Context


Verify commands:
- Discover and run the project compilation and linting suite to ensure derive macros expand without errors.
- Execute the test runner across data modeling test suites to validate serialization and conversion behaviors.

Accept when:
- Compilation succeeds with all derive attributes cleanly expanding across targeted data structures.
- All serialization and conversion test assertions pass without regression across boundary contracts.

## Enforcement

- Verified by: Automated continuous integration build checks validating trait derivation compilation
- Verified by: Peer code review verifying declarative derive macro adoption on boundary models
- Violation handling: Pull requests with manual boilerplate or missing trait derivations are blocked from merging pending revision.
- Exception process: Exceptions require documented justification for manual serialization submitted to core maintainers for review.