# Adoption of strum::EnumMessage for Enumeration Variant Metadata: Before Introducing Updating Strum Dependencies Consumer

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Multiple components across validation and input parsing require consistent textual descriptions for enumeration variants representing error conditions or parsing configurations.
- Hand-written pattern-matching functions to map enum variants to human-readable text increase boilerplate and risk becoming desynchronized when variants are added or modified.
- The strum library provides procedural macros and traits including strum::EnumMessage that attach message attributes directly to enum declarations.

## Problem Statement

Maintaining separate pattern-matching routines for enumerating human-readable error descriptions or variant messages across input parsing and validation boundaries introduces maintenance overhead and risks incomplete match statements when variants evolve. A declarative, type-level mechanism is required to bind metadata directly to enumeration definitions.

## Decision

1. MUST: Before introducing or updating strum dependencies, the consumer MUST locate the repository dependency manifest and lock artifact to resolve the exact locked version and verify API compatibility against the official version documentation.

## Policy Block

- MUST Before introducing or updating strum dependencies, the consumer MUST locate the repository dependency manifest and lock artifact to resolve the exact locked version and verify API compatibility against the official version documentation.

In scope:
- Input processing and validation enumeration types that expose variant-level descriptions or error messages
- Public contract boundaries where enum variants correspond to structured error messages or argument types

Out of scope:
- Internal numeric enumerations or bitflags that do not surface textual representations
- External foreign function interface boundaries that require primitive representation without procedural macro derivations

## Rationale

- Deriving strum::EnumMessage binds variant descriptions directly to the enum declaration, ensuring message updates are co-located with type definitions.
- Procedural derive macros eliminate repetitive match statements across validator and input modules, reducing boilerplate and preventing incomplete message mappings when new variants are introduced.
- The pattern aligns with observed usage across validation and JSON input processing contracts in the codebase.

## Consequences

Positive:
- Declarative enum messaging co-locates variant documentation and error messages directly with type definitions.
- Eliminates repetitive manual match functions, reducing code duplication across validators and parsers.
- Compiler checks ensure that message annotations adhere to the expected trait contracts during macro expansion.

Negative:
- Increases reliance on procedural macro compilation, which can add overhead to build times.
- Constrains variant messages to compile-time static strings rather than dynamic runtime formatting.

## Alternatives

- Manual implementation of custom display or message methods via match expressions (rejected)
  Rejected because: Manual match expressions require redundant boilerplate and lack compile-time guarantees that all variants maintain uniform message coverage when new variants are added.
  When valid: When dynamic runtime message formatting or localization is required for variant descriptions.
- Standard library Display trait implementations with format strings (rejected)
  Rejected because: Standard Display implementations conflate general string formatting with domain-specific message extraction and require manual match branching.
  When valid: When an enumeration requires standard formatting for general string printing rather than metadata extraction.

## Risks

- Macro compilation overhead accumulating across large enumeration sets.
  Mitigation: Limit derive usage to enumerations that actively participate in client-facing messaging or validation output.
  Owner: Engineering Team
- Upstream breaking changes in macro attribute syntax across dependency updates.
  Mitigation: Follow the lock-version grounding policy to verify API compatibility before applying dependency updates.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure that strum::EnumMessage derive attributes are placed directly on the enumeration declaration along with standard derive attributes.
- Verify that message access occurs via the methods provided by the strum::EnumMessage trait rather than ad-hoc string slicing.

## Continuation Context


Verify commands:
- Discover and execute the project's test suite to verify that enumeration message extraction behaves as expected across validation routines.
- Discover and run the project's static analysis and linting scripts to verify compliance with macro derive conventions.

Accept when:
- All enumerations requiring descriptive messages derive strum::EnumMessage without manual match dispatch routines.
- Project test suites and compilation checks pass cleanly with zero warnings or errors regarding enum message resolution.

## Enforcement

- Verified by: Automated continuous integration build and test pipelines
- Verified by: Peer code review during pull request evaluations
- Violation handling: Pull requests introducing manual match-based enum messaging will be requested to adopt strum::EnumMessage derivations.
- Exception process: Exceptions for enumerations requiring dynamic localization or non-static string generation must be approved by the architecture review team.