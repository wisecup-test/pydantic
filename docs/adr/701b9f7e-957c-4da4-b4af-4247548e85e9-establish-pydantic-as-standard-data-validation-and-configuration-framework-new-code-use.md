# Establish Pydantic as Standard Data Validation and Configuration Framework: New Code Use

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all Python codebases utilizing data validation, configuration management, or API contract enforcement. It governs the selection and usage of validation frameworks across backend services, CLI tools, and data processing pipelines.

## Context

- The codebase requires robust data validation and parsing capabilities, particularly for datetime handling, configuration management, and external client interactions
- Multiple files across the pydantic namespace (v1/datetime_parse.py, main.py, __init__.py, _internal/_config.py) demonstrate a consistent pattern of using Pydantic for data validation and schema enforcement
- External clients and API boundaries require type-safe contracts with automatic validation, serialization, and documentation generation
- The pattern shows integration with internal configuration systems (_internal/_config.py) indicating Pydantic is used as a foundational framework rather than an isolated utility
- Legacy v1 compatibility (v1/datetime_parse.py) suggests a deliberate migration strategy while maintaining backward compatibility

## Problem Statement

The system needs a standardized approach to data validation, type enforcement, and configuration management across multiple boundaries (external clients, internal services, configuration files). Without a consistent validation framework, teams may implement ad-hoc validation logic leading to inconsistent error handling, poor API contracts, security vulnerabilities from unvalidated input, and increased maintenance burden.

## Decision

1. SHOULD: New code SHOULD use Pydantic v2 APIs unless maintaining backward compatibility with v1 clients

## Policy Block

- SHOULD New code SHOULD use Pydantic v2 APIs unless maintaining backward compatibility with v1 clients

In scope:
- All REST API endpoints receiving or returning structured data
- Configuration file parsing (YAML, JSON, TOML)
- External client SDK interfaces
- Data transfer objects (DTOs) between service boundaries
- CLI argument parsing and validation
- Database model validation layers

Out of scope:
- Internal function parameter validation within a single module (use type hints only)
- Performance-critical hot paths where validation overhead is measured and unacceptable
- Legacy systems with established validation frameworks during migration period
- Simple scripts with no external interfaces

Exceptions:
- EXC-001: Performance profiling demonstrates Pydantic validation causes >10% latency increase in critical path
- EXC-002: Third-party library integration requires incompatible validation framework

## Rationale

- Pattern detected across 4 files with 91.15% confidence indicates established architectural practice rather than experimental usage
- Pydantic provides type-safe validation with automatic serialization/deserialization, reducing boilerplate and potential bugs
- Integration with FastAPI, SQLModel, and other modern Python frameworks creates ecosystem consistency
- The presence of both v1 and v2 code paths demonstrates intentional framework adoption with migration strategy
- Centralized validation logic in _internal/_config.py suggests Pydantic is used as foundational infrastructure

## Consequences

Positive:
- Consistent validation behavior across all API boundaries reduces integration bugs and improves developer experience
- Automatic OpenAPI/JSON Schema generation from Pydantic models enables self-documenting APIs
- Type safety catches validation errors at model definition time rather than runtime
- Reduced code duplication through reusable validation models and field validators
- Better error messages for clients through Pydantic's structured validation error responses

Negative:
- Additional dependency on Pydantic library increases application size and introduces upgrade maintenance burden
- Learning curve for developers unfamiliar with Pydantic's validation DSL and decorator patterns
- Potential performance overhead in high-throughput scenarios requiring careful profiling
- Migration effort required for existing codebases using alternative validation approaches
- Version compatibility challenges when maintaining both v1 and v2 Pydantic APIs simultaneously

## Alternatives

- Use Python dataclasses with manual validation logic (rejected)
  Rejected because: Dataclasses lack built-in validation, serialization, and parsing capabilities, requiring significant custom code that Pydantic provides out-of-box. This increases maintenance burden and bug surface area.
  When valid: Acceptable for internal data structures with no external boundaries and no validation requirements
- Use marshmallow for schema validation and serialization (rejected)
  Rejected because: Marshmallow separates schema definition from data classes, creating duplication. Pydantic's unified model approach with native type hints provides better IDE support and reduces boilerplate.
  When valid: May be considered for projects already heavily invested in marshmallow ecosystem
- Use attrs with validators for structured data (rejected)
  Rejected because: While attrs provides excellent data class functionality, it lacks Pydantic's JSON schema generation, parsing from various formats, and tight integration with modern web frameworks.
  When valid: Suitable for internal data structures where serialization and API integration are not required

## Risks

- Pydantic v1 to v2 migration may introduce breaking changes in existing validation logic
  Mitigation: Maintain v1 compatibility layer during transition period, implement comprehensive test coverage for validation logic, use pydantic.v1 namespace for legacy code
  Owner: Platform Engineering Team
- Performance degradation in high-throughput endpoints due to validation overhead
  Mitigation: Profile critical paths, use Pydantic's performance mode settings, implement caching for repeated validations, document exception process for performance-critical code
  Owner: Performance Engineering Team
- Complex nested validation rules may become difficult to debug and maintain
  Mitigation: Establish validation complexity guidelines, require code review for custom validators, provide team training on Pydantic best practices, maintain validation pattern library
  Owner: Engineering Team Leads

## Implementation Notes

- Start by defining Pydantic models for all new API endpoints and configuration objects before implementing business logic
- Use Pydantic's ConfigDict to set validation behavior (strict mode, arbitrary_types_allowed, etc.) consistently across the codebase
- Leverage Field() for adding constraints (min_length, max_length, regex patterns) and metadata (description, examples) to improve API documentation
- For datetime handling, use Pydantic's built-in datetime validators and configure timezone handling explicitly in model config
- When migrating from v1 to v2, use the migration guide and compatibility shims in pydantic.v1 namespace to minimize disruption

## Continuation Context


Verify commands:
- grep -r "from pydantic import" --include="*.py" | wc -l
- grep -r "class.*BaseModel" --include="*.py" | grep -v "test" | wc -l
- python -c "import pydantic; print(f'Pydantic version: {pydantic.VERSION}')"
- grep -r "@field_validator\|@model_validator" --include="*.py" | wc -l

Accept when:
- All API endpoint handlers use Pydantic models for request/response types (verified by grep showing BaseModel usage in route definitions)
- Configuration loading modules import from pydantic and define config classes as BaseModel subclasses
- Datetime parsing uses Pydantic validators rather than manual datetime.strptime calls
- Code review checklist includes verification that new external interfaces use Pydantic validation

## Enforcement

- Verified by: Automated CI checks scanning for BaseModel usage at API boundaries
- Verified by: Code review checklist requiring Pydantic models for new endpoints
- Verified by: Static analysis tools checking for manual validation logic that should use Pydantic
- Verified by: Architecture review for new services verifying validation framework choice
- Violation handling: CI pipeline fails if new API endpoints lack Pydantic model definitions
- Violation handling: Code review blocks merge if validation logic bypasses Pydantic without documented exception
- Violation handling: Architecture review required for any alternative validation framework introduction
- Violation handling: Quarterly audits identify non-compliant code for remediation planning
- Exception process: Submit exception request to architecture review board with performance data or technical justification
- Exception process: Document alternative approach and maintain equivalent type safety guarantees
- Exception process: Obtain tech lead approval and add exception to tracking system
- Exception process: Review exceptions annually to determine if they can be resolved with framework updates or optimization