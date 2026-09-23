# Adoption of jiter for JSON Input and Error Value Representation: Json Evaluation Pathways Not Convert Jiter

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- The validation engine processes high volumes of incoming JSON payloads that require strict schema conformance checking and detailed error tracking.
- Representing parsed JSON via intermediate host runtime structures incurs high allocation overhead and translation latency across boundary calls.
- Adopting jiter enables native handling of jiter::JsonValue and jiter::JsonObject directly inside input abstractions, validators, and error reporting structures without intermediate runtime object conversions.

## Problem Statement

Parsing and validating JSON inputs through generic runtime object bridges introduces substantial memory allocation overhead and conversion latency across validation boundaries. Without a unified native JSON data representation across input abstraction, field validation, and error reporting, the validation engine incurs redundant allocations and loses direct access to parsed JSON byte structures.

## Decision

1. MUST_NOT: JSON evaluation pathways MUST NOT convert jiter::JsonValue instances into intermediate dynamic foreign-function objects prior to schema validation.

## Policy Block

- MUST_NOT JSON evaluation pathways MUST NOT convert jiter::JsonValue instances into intermediate dynamic foreign-function objects prior to schema validation.

In scope:
- JSON input parsing and ingestion within core input abstraction layers
- Field validation routines processing structured JSON objects
- Error reporting pipelines capturing raw JSON input values

Out of scope:
- Non-JSON input validation pipelines processing native runtime objects
- External schema generation routines that do not process runtime JSON input

Exceptions:
- EXP-20-001: A validator handles non-JSON native runtime inputs that cannot be represented as jiter::JsonValue without loss of type identity

## Rationale

- Utilizing jiter::JsonValue and jiter::JsonObject across input abstraction and validator pipelines eliminates unnecessary allocation overhead during JSON validation passes.
- Embedding jiter::JsonValue directly within error line reporting ensures that problematic input values are accurately preserved without requiring synthetic host object reconstruction.
- Standardizing on jiter across all input-handling components creates a cohesive type contract that simplifies validator implementations and streamlines field traversal.

## Consequences

Positive:
- Direct JSON traversal without intermediary runtime object creation substantially reduces memory allocations and validation latency.
- Consistent use of jiter::JsonValue ensures precise error location and input value capture across validation failure paths.
- A unified JSON data contract simplifies the maintenance and extension of field validators and input adapters.

Negative:
- Validator implementations must maintain dual execution branches when supporting both jiter JSON types and host runtime objects.
- Tight coupling to jiter data structures requires careful version synchronization with the project lock artifact.

## Alternatives

- Converting all JSON inputs directly to host runtime objects before validation (rejected)
  Rejected because: Converting JSON data to host runtime objects introduces severe allocation overhead and slows down validation throughput significantly.
  When valid: When validation logic exclusively requires dynamic language introspection features not available in native compiled representations.
- Using general-purpose external serialization tree representations throughout validation (rejected)
  Rejected because: Generic serialization trees lack specialized iteration mechanisms optimized for zero-copy validation and tightly integrated error line reporting.
  When valid: When interoperability with third-party serialization frameworks is prioritized over raw validation throughput.

## Risks

- Breaking API changes in upstream jiter releases could destabilize input validation interfaces
  Mitigation: Enforce strict dependency lock-version grounding before updating the project dependency specifications
  Owner: Core validation engineering team
- Dual handling of jiter JSON values and native runtime types may cause implementation divergence in validators
  Mitigation: Maintain comprehensive cross-type conformance test suites verifying identical validation behavior across both representations
  Owner: Core validation engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Validators accepting structured inputs should unpack jiter::JsonObject entries directly into field extraction routines to minimize intermediate container copies.
- When constructing line-level error contexts, preserve the original jiter::JsonValue reference to allow precise formatting and serialization downstream.

## Continuation Context


Verify commands:
- Discover the project verification script from the manifest and execute the test suite covering JSON input validation pipelines.
- Discover and run the static type checking and linting tasks defined in the project build configuration to verify jiter type contract compliance.

Accept when:
- All test suites exercising jiter::JsonValue and jiter::JsonObject input parsing, field validation, and error reporting pass without regression.
- Static verification and linting processes confirm clean compilation with no unresolved type contract violations across validation modules.

## Enforcement

- Verified by: Continuous integration test passes verifying input handling, model field validation, and error reporting suites
- Verified by: Mandatory peer code review verifying adherence to jiter data structures for JSON evaluation paths
- Violation handling: Pull requests introducing non-jiter JSON representations or redundant intermediate runtime object conversions will be blocked during code review
- Exception process: Submit an architectural review request detailing why jiter data structures cannot satisfy the specific input representation requirements, accompanied by benchmark data