# Adoption of enum Module for String Enumerations and Configuration Protocols: Before Modifying Adding Enumeration Classes Relying

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Model configuration and input parsing require discrete categorical settings such as extra attribute handling and format protocol selection.
- Direct string literals scatter magic values across configuration structures, increasing the risk of typographical errors and inconsistent validation.
- Serialization routines and external callers require transparent string compatibility without adding custom serialization steps for every option type.

## Problem Statement

Relying on raw string literals for model configuration choices and serialization protocol selectors leads to typing inconsistencies, lack of compile-time verification, and runtime validation bugs across disparate modules.

## Decision

1. MUST: Before modifying or adding enumeration classes relying on external or runtime libraries, the consumer MUST inspect the dependency lock artifact to confirm target environment compatibility and API support.

## Policy Block

- MUST Before modifying or adding enumeration classes relying on external or runtime libraries, the consumer MUST inspect the dependency lock artifact to confirm target environment compatibility and API support.

In scope:
- Definition of categorical configuration parameters such as extra field handling policies.
- Specification of parsing protocol selectors and serialization format discriminators.
- Model configuration schemas requiring discrete option sets with string interoperability.

Out of scope:
- Open-ended user-defined string fields with dynamic or arbitrary values.
- Numeric identifiers or bitmask flags that do not require string representation.
- Internal boolean state toggles that represent binary conditions.

Exceptions:
- EXC-20-001: A third-party protocol or interface requires accepting arbitrary string values that cannot be bounded by a discrete enumeration.

## Rationale

- Subclassing Enum and the primitive string type provides strong static and runtime type checking while preserving transparent string behavior for serialization and comparison.
- Centralizing discrete options in explicit enumeration classes eliminates magic strings across configuration, parsing, and model validation modules.
- Dual inheritance from string and Enum enables serialization encoders to serialize option values directly without requiring specialized converters.

## Consequences

Positive:
- Eliminates hardcoded magic strings across configuration options and parser protocol dispatch routines.
- Enables compile-time and static analysis validation for categorical configuration keys and protocol arguments.
- Maintains transparent string comparison and serialization compatibility without custom serialization adapters.

Negative:
- Introduces additional class definitions and boilerplate compared to raw literal strings.
- Requires explicit value validation or member lookup when handling untrusted external string inputs.

## Alternatives

- Raw string constants or type alias unions of string literals without Enum classes (rejected)
  Rejected because: Raw string constants do not provide runtime member enumeration, type introspection, or centralized membership validation.
  When valid: When working in minimal environments where the runtime overhead of class definitions must be avoided.
- Standard integer or non-string Enum classes (rejected)
  Rejected because: Non-string enumerations require explicit string conversion during serialization and fail direct string equality checks against protocol inputs.
  When valid: When enumerating purely internal computational states or bitmask flags that require no external string representation.

## Risks

- Runtime overhead from repeated Enum member instantiation or dynamic lookup in high-throughput validation paths.
  Mitigation: Cache enumeration member references or enable configuration options that unwrap enum values during validation.
  Owner: Architecture and Core Engine Team
- Incompatibility when deserializing string inputs that do not strictly match predefined enumeration member values.
  Mitigation: Implement defensive type coercion in parsing routines to raise explicit type errors on unknown values.
  Owner: Data Parsing and Serialization Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Inherit from both str and Enum when defining categorical options that interface with serialization systems to preserve string compatibility.
- Use Enum member references in configuration dictionaries and contract signatures to ensure autocompletion and static type checking.

## Continuation Context


Verify commands:
- Discover the repository test runner from project configuration and execute the test suite covering configuration and parsing modules.
- Discover the repository type checking suite from project configuration and execute type analysis to verify enum member typing across model definitions.
- Discover the repository linting and static analysis configuration to verify adherence to enumeration usage standards.

Accept when:
- All configuration and parsing test suites execute successfully without enumeration value mismatches.
- Static type analysis passes with zero type errors regarding enumeration member assignments and parameter types.
- All defined string enumeration classes successfully serialize and deserialize through serialization encoders.

## Enforcement

- Verified by: Automated continuous integration pipelines executing test suites and static type checkers.
- Verified by: Peer code reviews verifying that new categorical options define string Enum classes rather than raw string literals.
- Violation handling: Pull requests introducing raw string literals for categorical configuration or protocol options will fail review and automated type verification.
- Violation handling: Violations identified in existing modules must be scheduled for refactoring to use standard string Enum classes.
- Exception process: Exceptions must be submitted via architecture review ticket specifying why dynamic string values are necessary over discrete enumeration members.
- Exception process: Approved exceptions must be documented in interface specifications and accompanied by explicit boundary validation logic.