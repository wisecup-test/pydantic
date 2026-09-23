# Adoption of dataclasses Module for Structured Data Modeling and Field Introspection: Documentation Utilities Use Dataclass Instances Format

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Internal subsystems require structured data models to manage schema references, deferred discriminator schemas, and field metadata.
- Support for introspecting user-defined structures demands consistent extraction routines across model fields and constructor signatures.
- The codebase relies on standard library dataclasses alongside typing annotations to define domain objects and facilitate schema transformation.

## Problem Statement

Inconsistent representations for internal schema accumulation and heterogeneous field introspection mechanisms across model types create maintenance overhead and potential inconsistencies in signature synthesis and schema resolution.

## Decision

1. MAY: Documentation utilities MAY use dataclass instances to format tabular and report outputs.

## Policy Block

- MAY Documentation utilities MAY use dataclass instances to format tabular and report outputs.

In scope:
- Internal modules responsible for schema gathering, field reflection, and constructor signature generation.
- Data conversion and reporting utilities constructing structured table rows.

Out of scope:
- External schema serialization layers implemented in lower-level binary modules.
- Runtime data validation executed outside high-level reflection routines.

## Rationale

- Adopting the standard dataclasses module provides a structured, type-safe representation for intermediate schema metadata without adding external dependencies.
- Centralizing field extraction in specialized routines like collect_dataclass_fields decouples model inspection from schema generation.
- Using inspect and signature_no_eval ensures parameter extraction without invoking untrusted runtime code.

## Consequences

Positive:
- Standardizes internal record representation using type-safe dataclass conventions across schema gathering and signature construction.
- Eliminates external runtime dependencies for structured metadata storage and reporting containers.
- Provides a unified introspection contract for extracting field metadata and parameter aliases.

Negative:
- Introspection over dataclass fields introduces runtime overhead during initial schema compilation.
- Internal maintenance overhead increases to maintain compatibility across both framework model classes and standard dataclass structures.

## Alternatives

- Ad-hoc dictionary structures for internal schema gathering and field collection (rejected)
  Rejected because: Untyped dictionaries lack static type verification and create operational fragility across multi-step schema traversal routines.
  When valid: Lightweight data passing where strict schema validation and attribute reflection are unnecessary.
- Custom proprietary record classes with manual equality and representation boilerplate (rejected)
  Rejected because: Manual class boilerplate increases code maintenance burden and duplicates standard library capabilities.
  When valid: Environments requiring specialized memory layouts or non-standard slot semantics incompatible with standard dataclass generation.

## Risks

- Variations in standard library dataclass behaviors across target environments
  Mitigation: Validate locked environment specifications and enforce automated continuous integration test suites across all supported runtime environments.
  Owner: Core Framework Engineering Team
- Performance bottlenecks during reflection-heavy field collection
  Mitigation: Cache gathered field metadata and avoid redundant signature evaluations during schema processing.
  Owner: Core Framework Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Introspect dataclass metadata and default values using standard field gathering helpers to ensure uniform schema compilation across model variants.
- Verify identifier validity on all alias and validation alias metadata when constructing signatures for dataclass constructors.

## Continuation Context


Verify commands:
- Discover the project test runner configuration from repository manifests and execute the test suite covering schema gathering, field extraction, and signature generation.
- Locate and run the repository static type analysis and linter scripts to verify dataclass usage and metadata typing compliance across internal modules.

Accept when:
- All automated test suites verifying dataclass field extraction, schema traversal, and signature generation execute successfully without errors.
- Static type analysis verifies that all field metadata structures and gathered schema references conform to defined type contracts without violations.

## Enforcement

- Verified by: Automated continuous integration test suites validating field extraction and schema generation.
- Verified by: Repository static type checking and linting pipelines verifying metadata type safety.
- Verified by: Peer code review for internal module contracts and field collectors.
- Violation handling: Pull requests introducing unvalidated or non-standard dataclass introspection patterns fail continuous integration verification.
- Violation handling: Non-compliant implementations must be refactored to utilize standard field collection utilities.
- Exception process: Exceptions require architectural review and approval from core framework maintainers documented in the issue tracker.