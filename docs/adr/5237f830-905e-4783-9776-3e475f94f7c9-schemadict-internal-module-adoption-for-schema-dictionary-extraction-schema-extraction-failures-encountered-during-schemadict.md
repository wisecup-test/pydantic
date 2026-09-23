# SchemaDict Internal Module Adoption for Schema Dictionary Extraction: Schema Extraction Failures Encountered During Schemadict

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Core engine components require structured schema extraction to instantiate validators, serializers, and configuration handlers from dictionary definitions.
- Direct manipulation of untyped dictionary structures across separate validator and serializer implementations risks fragmented error reporting and inconsistent type coercion.
- The codebase establishes an internal SchemaDict module to unify dictionary property extraction and schema error propagation across compilation boundaries.

## Problem Statement

Instantiating validator and serializer structures directly from untyped dictionary inputs introduces redundant key lookup logic, divergent type validation, and inconsistent error construction across disparate subsystem builders. A unified schema dictionary extraction module is required to guarantee uniform contract enforcement and centralized schema error generation during builder initialization.

## Decision

1. MUST: Schema extraction failures encountered during SchemaDict parsing MUST propagate through the centralized schema error construction pipeline.

## Policy Block

- MUST Schema extraction failures encountered during SchemaDict parsing MUST propagate through the centralized schema error construction pipeline.

In scope:
- Construction of validation components from core schema dictionary inputs.
- Construction of serializer components and serialization configurations from core schema dictionary inputs.
- Initialization of prebuilt schema references and reusable core definitions.

Out of scope:
- Runtime data validation execution against already-constructed validator graphs.
- Serialization transformations operating directly on output data buffers without schema dictionary parsing.

## Rationale

- Static IR evidence confirms uniform adoption of SchemaDict across prebuilt routines, serializer builders, configuration loaders, and validator modules.
- Centralizing dictionary access within SchemaDict standardizes extraction semantics, schema error generation, and interning operations across all core builders.
- Enforcing a shared schema extraction abstraction prevents parsing discrepancies between validation logic and serialization routines.

## Consequences

Positive:
- Guarantees consistent schema validation error messages across all validator and serializer construction points.
- Eliminates duplicate dictionary extraction and type checking routines across independent builder modules.
- Simplifies schema structure updates by maintaining a single schema extraction interface.

Negative:
- Couples all validator and serializer builders to the internal SchemaDict interface design.
- Introduces an abstraction layer over direct dictionary accesses, requiring builders to conform to pre-established access patterns.

## Alternatives

- Direct dictionary key inspection within each builder module (rejected)
  Rejected because: Results in widespread code duplication, inconsistent missing-key handling, and disparate error construction across builders.
  When valid: Only in isolated single-file prototypes that have no dependencies on standard schema definitions.
- Complete deserialization into intermediate statically typed struct representations (rejected)
  Rejected because: Incurs additional allocation and conversion overhead for complex polymorphic schemas with optional fields.
  When valid: When schemas are fixed, non-polymorphic, and static without conditional branch requirements.

## Risks

- Changes to SchemaDict methods could force simultaneous updates across all dependent validator and serializer builders.
  Mitigation: Maintain stable extraction method signatures and introduce deprecation paths for modified schema accessors.
  Owner: Core Engineering Team
- Incomplete SchemaDict coverage might tempt developers to bypass the abstraction for niche schema properties.
  Mitigation: Expand SchemaDict helper capabilities when new schema key formats are introduced rather than permitting direct dictionary lookups.
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
- Implement new schema property extraction helpers directly within the SchemaDict trait or struct implementation before consuming them in builders.
- Ensure all schema parsing failures in SchemaDict invoke the common schema error constructor to maintain uniform diagnostic messages.

## Continuation Context


Verify commands:
- Discover and execute the project compilation and linting suite to ensure all schema extraction points conform to SchemaDict contracts.
- Discover and run the project test harness covering validator and serializer builder construction to verify schema parsing compliance.

Accept when:
- All builder tests pass with all schema extractions performed through SchemaDict.
- Static analysis and linters report zero direct raw dictionary key lookups in validator and serializer construction routines.

## Enforcement

- Verified by: Continuous integration test workflows.
- Verified by: Automated static analysis and code review checks.
- Violation handling: Pull requests containing raw dictionary lookups in builder routines will be blocked from merging until refactored to use SchemaDict.
- Exception process: Exceptions require documented justification demonstrating why SchemaDict cannot fulfill the extraction requirement and approval from the core architecture maintainers.