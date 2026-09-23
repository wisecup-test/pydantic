# Pydantic BaseModel Schema Deserialization for External GraphQL API Protocols: Hierarchical Comment Discussion Reply Structures Compose

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- External integration components interact with public GraphQL APIs to ingest repository discussions, comments, and contributor interactions.
- Public GraphQL endpoints return heterogeneous, deeply nested JSON responses containing edges, nodes, timestamps, and nullable user profile fields.
- Consuming raw untyped dictionaries directly leads to runtime attribute errors and brittle access logic across data processing pipelines.
- HTTP communication with external services requires explicit status verification, error inspection, and credential secret masking.

## Problem Statement

External GraphQL endpoints return complex, deeply nested JSON payloads that may contain absent fields, null author entities, or top-level query errors. Consuming external payloads without contract enforcement introduces runtime type failures, complicates defensive dictionary access, and couples downstream data aggregation routines directly to external wire formats.

## Decision

1. SHOULD: Hierarchical comment and discussion reply structures SHOULD compose node models through container schemas declaring typed list collections.

## Policy Block

- SHOULD Hierarchical comment and discussion reply structures SHOULD compose node models through container schemas declaring typed list collections.

In scope:
- Routines that execute HTTP requests against external GraphQL endpoints and process external entity responses.
- Data ingestion workflows that traverse nested discussion, comment, or contributor structures returned by third-party APIs.

Out of scope:
- Internal inter-module communication within the local repository codebase.
- Simple webhook dispatch routines that forward uninspected binary or string payloads directly to external sinks.

Exceptions:
- EXC-25-001: An external API returns raw unstructured text or binary payloads that cannot be mapped into structured schemas.

## Rationale

- Subclassing BaseModel guarantees deterministic validation, automatic date-time parsing, and immediate failure when external API schemas undergo breaking changes.
- Encapsulating protocol schemas in typed models separates the volatile external GraphQL representation from downstream domain data structures.
- Inspecting GraphQL response error blocks prior to deserialization prevents cryptic parsing exceptions when partial errors accompany successful HTTP status codes.
- Secret-masking accessors ensure external API credentials do not leak into diagnostic logs or standard output.

## Consequences

Positive:
- Guarantees strong type safety and explicit validation boundaries for all incoming external GraphQL payloads.
- Eliminates defensive dictionary parsing and reduces runtime KeyError exceptions across contributor data processing.
- Centralizes API contract modifications into declarative schema definitions.

Negative:
- Requires defining and maintaining explicit schema classes for every external GraphQL query and mutation payload.
- Adds validation runtime overhead during bulk deserialization of deeply nested discussion and comment trees.

## Alternatives

- Direct dictionary indexing with manual defensive checks (rejected)
  Rejected because: Manual dictionary access lacks schema validation, produces verbose nested guard clauses, and fails silently on malformed external responses.
  When valid: One-off lightweight scripts with trivial, flat JSON payloads.
- Generic TypedDict definitions without runtime validation (rejected)
  Rejected because: TypedDict provides static type hints during development but performs no runtime validation against breaking changes or null values from external endpoints.
  When valid: Environments where runtime validation libraries are strictly disallowed due to minimal dependency constraints.

## Risks

- External GraphQL API schema evolution can cause runtime validation failures if the external service introduces breaking schema changes.
  Mitigation: Include optional type fallbacks for nullable fields and log complete API error responses when deserialization fails.
  Owner: Integration Engineering Team
- Performance degradation when deserializing large arrays of deeply nested comment nodes in high-volume repository interactions.
  Mitigation: Paginate GraphQL queries using cursor boundaries and deserialize only requested fields required for downstream domain calculation.
  Owner: Integration Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Model GraphQL edge and node hierarchies by composing parent container classes around lists of nested child models.
- Always configure external HTTP client calls with explicit request timeouts and verify both HTTP status codes and response error fields before data extraction.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute the test suite covering external API client schemas.
- Execute the repository linting and static type checking directives defined in the project configuration to confirm schema conformance.

Accept when:
- All unit and integration tests for external GraphQL schema deserialization pass with zero validation errors.
- Static type analysis confirms that all fields accessed in downstream processing routines align with declared model attributes.

## Enforcement

- Verified by: Continuous integration automated test suite executing mock external response validation.
- Verified by: Peer code review verifying that all new external API endpoints declare corresponding BaseModel schemas.
- Violation handling: Pull requests containing untyped dictionary access to external API responses will be blocked until schema models are introduced.
- Exception process: Submit an architectural review request outlining why structured schema modeling cannot accommodate the external payload format.