# Adoption of annotated_types for Canonical Type Constraint Metadata Representation: Validation Transformation Pipeline Abstractions Compose Constraint

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Type validation systems require a standardized, decoupled mechanism to express scalar, collection, and temporal boundaries within type annotations.
- Embedding proprietary constraint objects directly into type annotations leads to tight coupling between domain models and specific validation engines.
- The adoption of standard type metadata protocols enables uniform constraint parsing across field reflection, experimental pipeline chaining, and specialized type constructs.

## Problem Statement

Representing field constraints and validation boundaries with disparate, framework-specific metadata objects creates fragmentation across field definitions, pipeline step composers, and custom type aliases, hindering interoperability with standard type introspection mechanisms.

## Decision

1. MUST: Validation and transformation pipeline abstractions MUST compose constraint steps using `annotated_types` predicate and boundary structures rather than custom ad-hoc predicates.

## Policy Block

- MUST Validation and transformation pipeline abstractions MUST compose constraint steps using `annotated_types` predicate and boundary structures rather than custom ad-hoc predicates.

In scope:
- Field configuration metadata collection and constraint mapping
- Validation pipeline step composition and constraint chaining
- Custom scalar and temporal type constraint declarations

Out of scope:
- Internal core engine schemas that bypass standard metadata annotation layers
- Untyped runtime dictionaries and legacy schema conversion utilities

## Rationale

- Adopting annotated_types establishes an interoperable contract for standard constraints such as boundary limits, length, intervals, and predicates across all field representations.
- Centralizing constraint descriptors into standard metadata classes simplifies schema reconstruction and annotation inspection without duplicating constraint logic across modules.
- Chaining constraints within execution pipelines through standard metadata contracts preserves consistent validation semantics across procedural and declarative type definitions.

## Consequences

Positive:
- Provides a standard, portable representation of constraint metadata compatible with standard type annotations.
- Reduces code duplication across field definitions, specialized types, and transformation pipelines by sharing common constraint descriptors.
- Enables uniform schema extraction and annotation reconstruction across all model fields.

Negative:
- Introduces an external dependency coupling for fundamental constraint representations.
- Requires translation and mapping layers between legacy keyword arguments and metadata class instantiations.

## Alternatives

- Implement proprietary constraint metadata classes within internal typing modules (rejected)
  Rejected because: Creating custom constraint classes isolates metadata handling from broader typing ecosystem standards and duplicates constraint validation definitions.
  When valid: Valid only in environments where external library dependencies are strictly prohibited.
- Store constraint definitions as untyped keyword dictionaries on field descriptors (rejected)
  Rejected because: Untyped constraint dictionaries lack static type safety, impede declarative pipeline composition, and complicate annotation reconstruction.
  When valid: Valid only for dynamic schema generation where types are not known at analysis time.

## Risks

- Divergence between supported constraint keywords in field declarations and available metadata descriptors
  Mitigation: Maintain explicit lookup tables that validate metadata constructor mappings against supported parameters.
  Owner: Core Architecture Team
- Overhead of object instantiation when converting constraint keywords to metadata instances
  Mitigation: Utilize lightweight metadata structures with fast initialization paths and reuse immutable descriptor instances where feasible.
  Owner: Core Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Constraint keyword mappings should be consolidated in a centralized metadata lookup table to ensure consistent translation across all field creation pathways.
- Pipeline constraint implementations should delegate evaluation to canonical metadata predicate structures rather than re-implementing comparison logic.

## Continuation Context


Verify commands:
- Discover the project test runner and execute the test suite covering field metadata extraction and constraint validation.
- Discover the static analysis suite and verify type checking compliance across modules importing constraint metadata.

Accept when:
- All field constraint arguments correctly instantiate and serialize into standard metadata descriptors.
- Pipeline constraint methods successfully construct execution chains using standard constraint descriptors without runtime type errors.
- Introspection and annotation reconstruction suites pass without regressions across all field configurations.

## Enforcement

- Verified by: Automated test suites validating field metadata collection and constraint evaluation
- Verified by: Static type analysis validating type annotations and constraint descriptors
- Verified by: Peer code review for all new field configuration options or pipeline operations
- Violation handling: Rejection of pull requests that introduce proprietary constraint descriptors where standard metadata classes exist
- Violation handling: Static analysis and test failures blocking integration pipelines until constraint mappings conform to the standard metadata protocol
- Exception process: Submit an architectural review request detailing novel constraint requirements that cannot be represented by existing metadata descriptors.
- Exception process: Document temporary proprietary metadata classes with an upstream contribution roadmap to integrate missing constraints into the standard metadata protocol.