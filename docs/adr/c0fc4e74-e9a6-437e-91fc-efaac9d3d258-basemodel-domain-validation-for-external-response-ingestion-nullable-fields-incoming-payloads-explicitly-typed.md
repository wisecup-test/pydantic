# BaseModel Domain Validation for External Response Ingestion: Nullable Fields Incoming Payloads Explicitly Typed

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- External data fetched from remote services arrives as dynamic hierarchical structures requiring parsing and type validation.
- Direct indexing of raw response mappings leads to runtime errors and fragile data access paths across downstream logic.
- Domain modeling with BaseModel establishes explicit schemas for entities during data ingestion boundaries.

## Problem Statement

Ingesting unvalidated external API response structures creates runtime fragility when fields are missing or types mutate unexpectedly. Without structured schema validation at the ingestion boundary, downstream consumers must perform defensive checks across all access paths.

## Decision

1. SHOULD: Nullable fields in incoming payloads SHOULD be explicitly typed with optional unions to prevent schema validation failures on missing data.

## Policy Block

- SHOULD Nullable fields in incoming payloads SHOULD be explicitly typed with optional unions to prevent schema validation failures on missing data.

In scope:
- Ingestion and parsing of structured external API response payloads.
- Domain entity modeling for external data structures passed to downstream business logic.

Out of scope:
- Scalar configuration and environment variable lookups.
- Internal data representations within modules that do not cross service or external boundaries.

Exceptions:
- EXC-56-001: External payloads contain arbitrary dynamic metadata that cannot be mapped to predefined structural schemas.

## Rationale

- Subclassing BaseModel provides runtime verification and type validation for nested external response nodes.
- Explicit schemas for domain entities clarify data contracts between external endpoints and internal processing routines.
- Structured modeling guarantees attribute availability and prevents malformed data from propagating to business logic.

## Consequences

Positive:
- Guarantees structural and type validation at the ingestion boundary for external payloads.
- Provides self-documenting domain model structures with typed attribute access for downstream consumers.
- Prevents malformed external response payloads from propagating across domain processing routines.

Negative:
- Requires defining and maintaining explicit domain model classes alongside external interface contracts.
- Introduces minor execution overhead during payload deserialization and validation.

## Alternatives

- Direct untyped associative mapping traversal for external payload ingestion (rejected)
  Rejected because: Lacks structural validation and exposes consumers to runtime key errors and data inconsistencies upon upstream payload changes.
  When valid: Temporary scripts where schema stability and validation guarantees are unnecessary.
- Standard language data structures without runtime type validation logic (rejected)
  Rejected because: Does not validate runtime types or nested payload structures upon deserialization of external inputs.
  When valid: Internal data transfers where structures have already been strictly validated at an earlier boundary.

## Risks

- External schema drift may trigger deserialization validation errors at runtime.
  Mitigation: Define nullable payload attributes using optional type annotations to accommodate expected schema variances.
  Owner: Engineering Team
- Maintenance overhead from defining extensive domain models for peripheral external integrations.
  Mitigation: Scope domain models strictly to attributes accessed by application logic rather than mapping entire external contracts.
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
- Define domain model attributes to match external interface fields, restricting definitions to properties utilized by downstream routines.
- Record diagnostic error output when external responses indicate communication failures or payload validation issues.

## Continuation Context


Verify commands:
- Discover and execute the project verification script from the repository manifest to run domain validation tests.
- Discover and execute the repository static analysis suite to verify model type annotations and boundary contracts.

Accept when:
- External data ingestion routines validate incoming structured payloads through declared domain models inheriting from BaseModel.
- Repository static analysis suites and test suites execute successfully without schema or type validation errors.

## Enforcement

- Verified by: Automated static type checking and continuous integration pipelines.
- Verified by: Peer code reviews evaluating payload ingestion and domain modeling patterns.
- Violation handling: Pull requests that ingest external structured payloads without domain model validation are blocked pending schema definition.
- Exception process: Submit an architecture exception request documenting why raw unstructured traversal is strictly required for the specific payload.
- Exception process: Obtain approval from domain maintainers before merging unvalidated data access implementations.