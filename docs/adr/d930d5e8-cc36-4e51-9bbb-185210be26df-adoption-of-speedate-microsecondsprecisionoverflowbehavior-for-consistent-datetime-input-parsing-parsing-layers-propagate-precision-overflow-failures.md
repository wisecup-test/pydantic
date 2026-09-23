# Adoption of speedate MicrosecondsPrecisionOverflowBehavior for Consistent Datetime Input Parsing: Parsing Layers Propagate Precision Overflow Failures

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Input validation layers process temporal data across distinct payload representations, including raw string tokens and structured JSON values.
- Sub-microsecond timestamps require explicit, uniform rules for handling fractional precision overflow to prevent inconsistent parsing behavior between formats.
- The input processing modules import speedate and specifically consume speedate::MicrosecondsPrecisionOverflowBehavior to govern fractional second validation.

## Problem Statement

Parsing datetime values from diverse input representations requires uniform handling of fractional seconds. Without a standardized precision overflow policy, disparate input adapters risk divergent parsing semantics, silent rounding errors, or unhandled precision overflow when processing high-precision temporal inputs.

## Decision

1. SHOULD: Parsing layers SHOULD propagate precision overflow failures as structured input validation errors rather than panicking or silently truncating fractional second values.

## Policy Block

- SHOULD Parsing layers SHOULD propagate precision overflow failures as structured input validation errors rather than panicking or silently truncating fractional second values.

In scope:
- All input validation modules responsible for parsing temporal strings or structured JSON fields into datetime structures.
- Any data transformation routines encountering sub-microsecond timestamp representations.

Out of scope:
- Non-temporal scalar string and numeric parsing routines.
- High-level schema generation logic that does not directly parse or coerce runtime payload values.

## Rationale

- Evidence across input parsing modules shows consistent reliance on speedate::MicrosecondsPrecisionOverflowBehavior to govern fractional precision handling.
- Centralizing datetime parsing configuration on speedate prevents behavioral discrepancies between string-based and JSON-based input payloads.
- Adopting explicit precision overflow controls ensures predictability when handling temporal data exceeding standard microsecond resolution.

## Consequences

Positive:
- Guarantees consistent temporal parsing semantics and precision overflow behavior across heterogeneous input formats.
- Eliminates redundant datetime parsing logic by delegating high-performance parsing to a specialized library.
- Ensures sub-microsecond precision boundaries are handled deterministically without silent truncation or arithmetic panics.

Negative:
- Input parsing routines become coupled to the specific API semantics and error types exposed by the adopted datetime parsing library.
- Any upstream changes in precision overflow handling or enum variants necessitate coordinated updates across all consuming input adapters.

## Alternatives

- In-house custom datetime parsing logic implemented independently within each input parsing module (rejected)
  Rejected because: Increases maintenance burden, introduces risk of divergent datetime parsing behavior between string and structured JSON inputs, and duplicates parsing logic.
  When valid: Valid only when external dependencies are strictly prohibited and temporal format requirements are trivial.
- Standard library or generic scalar parsing without explicit precision overflow handling (rejected)
  Rejected because: Standard conversions lack configurable sub-microsecond precision truncation policies, risking silent data corruption or inconsistent timestamp resolutions.
  When valid: Valid when timestamps never exceed millisecond granularity or strict precision control is non-critical.

## Risks

- Discrepancies in precision overflow behavior between upstream library updates and downstream expectations.
  Mitigation: Cover all precision boundary conditions with comprehensive unit test suites comparing string and structured input formats.
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
- Configure datetime parsing invocations to pass uniform precision overflow settings across all input source adapters.
- Map library-specific precision overflow parse errors directly into standard validation error responses.

## Continuation Context


Verify commands:
- Discover the workspace dependency configuration and build verification script from the repository root, then execute the standard test suite to confirm datetime parsing behavior conforms to specification.
- Inspect the project validation commands to run type checking and static analysis across all input parsing units.

Accept when:
- All test suites validating temporal input parsing across string and structured object formats pass without regressions.
- Sub-microsecond timestamp inputs trigger deterministic overflow handling in accordance with the configured precision overflow behavior across all input parsing boundaries.
- Static verification and code checks across input parsing modules confirm that external datetime parsing types match resolved dependency specifications.

## Enforcement

- Verified by: Automated test execution across input parsing modules covering timestamp edge cases and overflow conditions.
- Verified by: Peer code reviews ensuring all temporal input parsing delegates to standard precision overflow configurations.
- Violation handling: Pull requests introducing ad-hoc datetime parsing or bypassing precision overflow settings are blocked until aligned with standardized library usage.
- Violation handling: Violations detected during automated verification fail build pipelines.
- Exception process: Deviations from standard precision overflow handling require an architectural review detailing why format-specific parsing is required.
- Exception process: Approved exceptions must document expected precision differences and include dedicated compatibility test suites.