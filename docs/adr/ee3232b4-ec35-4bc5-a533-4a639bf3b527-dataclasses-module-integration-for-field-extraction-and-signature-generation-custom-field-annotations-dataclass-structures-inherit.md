# dataclasses Module Integration for Field Extraction and Signature Generation: Custom Field Annotations Dataclass Structures Inherit

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Internal schema construction requires extracting structural field definitions from both models and dataclass containers without altering class definitions at import time.
- Runtime constructor generation depends on reflective inspection to align callable parameter specifications with defined field metadata and aliases.
- The introspection pipeline isolates field extraction contracts such as collect_dataclass_fields and collect_model_fields from the dynamic signature synthesizer generate_pydantic_signature.

## Problem Statement

Internal metadata extraction and constructor generation routines require uniform introspection of standard dataclass structures without compromising custom model reflection or alias resolution. Without dedicated contracts linking dataclass field extraction to signature synthesis, callable parameter introspection fails to reflect field aliases, validation constraints, and default values established on dataclass instances.

## Decision

1. SHOULD: Custom field annotations on dataclass structures SHOULD inherit from PydanticMetadata or wrap metadata using pydantic_general_metadata to preserve introspection attributes across model rebuilds.

## Policy Block

- SHOULD Custom field annotations on dataclass structures SHOULD inherit from PydanticMetadata or wrap metadata using pydantic_general_metadata to preserve introspection attributes across model rebuilds.

In scope:
- Field extraction and metadata collection routines processing dataclasses and models.
- Dynamic callable signature synthesis for model and dataclass initializers.

Out of scope:
- Third-party schema serialization formats handled outside reflective field extraction.
- Runtime validation execution delegated entirely to low-level core validators.

## Rationale

- Separating collect_dataclass_fields from collect_model_fields isolates standard library dataclass reflection logic from custom model configuration mechanisms.
- Dynamic signature creation via generate_pydantic_signature ensures that constructor signatures accurately reflect aliases and parameter validation requirements without mutating the underlying class.
- Abstracting metadata preservation through PydanticMetadata and pydantic_general_metadata ensures consistency between dataclass fields and framework field definitions during introspection.

## Consequences

Positive:
- Unifies field extraction interfaces across standard dataclasses and custom models.
- Generates accurate callable signatures that respect field aliases and validation parameters.
- Prevents premature evaluation of forward references and annotations during signature synthesis.

Negative:
- Increases introspection overhead when dynamically generating constructor signatures.
- Requires continuous alignment between dataclass field extraction contracts and signature generation routines.

## Alternatives

- Direct inspection via inspect.signature without dedicated dataclass field extraction (rejected)
  Rejected because: Fails to account for field aliases, validation metadata, and custom field overrides present on dataclass structures.
  When valid: When inspecting pure callables that do not carry schema metadata or field alias mappings.
- Converting all dataclasses into custom model definitions prior to reflection (rejected)
  Rejected because: Alters class inheritance structures and violates non-intrusive interoperability requirements for standard library dataclasses.
  When valid: When standard dataclass compatibility is deprecated in favor of unified base models.

## Risks

- Changes in standard library dataclass introspection behavior across language runtimes could desynchronize field collection.
  Mitigation: Encapsulate standard library reflection calls within collect_dataclass_fields behind regression test suites.
  Owner: engineering team
- Dynamic signature synthesis could introduce performance penalties during high-frequency class creation.
  Mitigation: Cache synthesized signatures and defer signature generation until inspection is explicitly requested.
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
- Implementers must ensure that collect_dataclass_fields returns field mappings compatible with the metadata expectations of generate_pydantic_signature.
- Verification of parameter identifiers must precede signature parameter construction to avoid syntax errors with non-standard field names.

## Continuation Context


Verify commands:
- Discover the test runner configuration from the repository manifest and execute the test suite covering field reflection and dataclass extraction.
- Locate the static analysis configuration and run the type checker and linter across modules implementing signature synthesis contracts.

Accept when:
- Dataclass field extraction correctly populates field metadata structures with aliases and validation markers.
- Generated constructor signatures match dataclass parameter definitions without raising evaluation errors.
- All discovered test suites and static analysis checks pass with zero violations.

## Enforcement

- Verified by: Automated continuous integration test suites validating field extraction and signature reflection.
- Verified by: Peer review enforcing architectural boundaries between field collection and signature generation.
- Violation handling: Pull requests bypassing collect_dataclass_fields or generate_pydantic_signature must be blocked until compliant.
- Violation handling: Non-compliant reflection implementations must be refactored to adhere to defined contracts.
- Exception process: Exceptions must be submitted via an architecture review proposal detailing technical constraints and alternatives considered.