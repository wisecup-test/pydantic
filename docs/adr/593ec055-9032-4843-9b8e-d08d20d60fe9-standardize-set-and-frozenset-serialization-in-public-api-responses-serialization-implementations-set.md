# Standardize Set and FrozenSet Serialization in Public API Responses: Serialization Implementations Set

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Public APIs require consistent and predictable serialization behavior to ensure client compatibility across different programming languages and platforms
- Python's set and frozenset types are unordered collections that lack a standard JSON representation, creating ambiguity in API contracts
- The codebase implements custom serialization logic for set and frozenset types in both Rust (pydantic-core) and Python (pydantic validators), indicating a deliberate architectural choice
- External API consumers expect deterministic output formats that can be reliably parsed and validated against schemas
- The pattern appears in serialization and validation layers, suggesting this is a cross-cutting concern affecting the entire API surface

## Problem Statement

When exposing Python set and frozenset types through public APIs, there is no native JSON representation for these unordered collection types. Without a standardized serialization approach, different parts of the system may serialize these types inconsistently, leading to API contract violations, client integration failures, and unpredictable behavior across language boundaries.

## Decision

1. MUST: Serialization implementations for set and frozenset types MUST be consistent across all serialization layers (Rust core and Python validators)

## Policy Block

- MUST Serialization implementations for set and frozenset types MUST be consistent across all serialization layers (Rust core and Python validators)

In scope:
- All public-facing REST API endpoints
- GraphQL API responses containing set or frozenset fields
- Webhook payloads and event streams
- API response schemas and OpenAPI specifications
- Client SDK serialization behavior

Out of scope:
- Internal service-to-service communication using binary protocols
- Database storage representations
- In-memory data structures used within application logic
- Debug or diagnostic endpoints not intended for public consumption

Exceptions:
- EXC-001: Legacy API versions (v1.x) that have documented set serialization behavior different from this standard

## Rationale

- The pattern was detected in 2 critical files (pydantic-core serializers and pydantic validators) with 90% significance, indicating this is an established architectural pattern
- JSON specification (RFC 8259) does not define a native set type, making array serialization the most compatible choice for cross-platform interoperability
- Implementing consistent serialization at both the Rust core and Python validation layers ensures uniform behavior regardless of code path or optimization level
- Standardizing on array representation aligns with common practices in other API frameworks and reduces cognitive load for API consumers

## Consequences

Positive:
- API responses become predictable and consistent across all endpoints, improving client reliability
- Cross-language compatibility is maximized since arrays are universally supported in JSON parsers
- Schema validation and OpenAPI documentation generation become straightforward
- Client libraries can be generated automatically with correct type mappings

Negative:
- Set semantics (uniqueness, unordered nature) are not explicitly enforced in the JSON representation
- Clients must understand that array order is not guaranteed and may vary between responses
- Potential performance overhead if sorting is implemented for deterministic output
- Legacy clients expecting different serialization formats may require migration

## Alternatives

- Serialize sets as JSON objects with elements as keys and null/true as values (rejected)
  Rejected because: This approach is non-standard, increases payload size unnecessarily, and creates confusion about the semantic meaning of the values
  When valid: Only valid for internal APIs where both producer and consumer explicitly agree on this format
- Use custom MIME type (application/vnd.api+set) with specialized serialization (rejected)
  Rejected because: Requires custom client support, breaks compatibility with standard HTTP clients, and adds unnecessary complexity
  When valid: Could be considered for specialized high-performance internal services with custom client libraries
- Convert sets to lists at the model boundary before serialization (rejected)
  Rejected because: Loses type information and makes validation logic more complex; serializer-level handling is cleaner
  When valid: Acceptable for simple applications without sophisticated validation requirements

## Risks

- Clients may incorrectly assume array order is stable and write brittle integration code
  Mitigation: Explicitly document in API specifications that order is not guaranteed; include randomization in test fixtures
  Owner: API Documentation Team
- Large sets may create performance issues during serialization, especially if sorting is applied
  Mitigation: Implement performance benchmarks for set serialization; consider pagination for endpoints returning large collections
  Owner: Engineering Team
- Duplicate elements in deserialized arrays may not be properly handled by all client implementations
  Mitigation: Implement server-side validation to reject arrays with duplicates when deserializing to sets; provide clear error messages
  Owner: API Validation Team

## Implementation Notes

- Ensure both pydantic-core (Rust) and pydantic validators (Python) use identical serialization logic to prevent inconsistencies
- Add integration tests that verify set serialization behavior across different API endpoints and response types
- Update OpenAPI schema generation to correctly represent set/frozenset fields as arrays with uniqueItems constraint
- Consider implementing a configuration flag to enable deterministic sorting for testing environments while keeping production unsorted for performance

## Continuation Context


Verify commands:
- grep -r 'set.*serializ' pydantic-core/src/serializers/ pydantic/validators.py | grep -v test
- python -c "from pydantic import BaseModel; from typing import Set; class M(BaseModel): s: Set[int]; import json; print(json.loads(M(s={3,1,2}).json())['s'])"
- rg 'type_serializers/(set|frozenset)' --type rust

Accept when:
- All set and frozenset fields in API responses serialize to valid JSON arrays
- Serialization behavior is identical between Rust core and Python validator implementations
- OpenAPI schemas correctly represent set/frozenset types with array type and uniqueItems constraint

## Enforcement

- Verified by: Automated integration tests in CI pipeline that validate serialization output format
- Verified by: OpenAPI schema validation in pre-deployment checks
- Verified by: Code review checklist item for any new API endpoints exposing set/frozenset types
- Violation handling: CI pipeline fails if serialization tests detect non-array output for set/frozenset types
- Violation handling: API gateway validation rejects responses that don't match OpenAPI schema
- Violation handling: Runtime monitoring alerts on serialization errors or type mismatches
- Exception process: Submit exception request to API Architecture Team with justification and impact analysis
- Exception process: Document exception in API changelog and endpoint-specific documentation
- Exception process: Set sunset date for exception with migration plan to standard behavior