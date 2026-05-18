# Enforce Domain Validation in Public API Models: Public External Endpoints

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all public/external API endpoints and data models exposed to external consumers.

## Context

- Public APIs serve as the primary interface between external consumers and internal systems, requiring robust validation to prevent malformed data from entering the system
- The codebase demonstrates extensive use of type checking, validation frameworks (Pydantic), and mypy plugin configurations across 75 files, indicating a strong emphasis on type safety and domain validation
- External API consumers may send invalid, malicious, or unexpected data that could compromise system integrity, data quality, or security if not properly validated
- The pattern appears in test files, type checking modules, root models, and plugin configurations, suggesting validation is a cross-cutting concern applied consistently across the API surface
- Domain validation at API boundaries provides early failure detection, clear error messages to consumers, and prevents invalid state propagation through the system

## Problem Statement

Public and external APIs lack consistent, comprehensive domain validation at entry points, creating risk of invalid data entering the system, poor error messages for API consumers, and potential security vulnerabilities. Without standardized validation patterns, each API endpoint may implement validation inconsistently or incompletely, leading to data quality issues and increased maintenance burden.

## Decision

1. MUST: All public/external API endpoints MUST validate input data using strongly-typed models with explicit domain validation rules before processing requests

## Policy Block

- MUST All public/external API endpoints MUST validate input data using strongly-typed models with explicit domain validation rules before processing requests

In scope:
- All REST API endpoints exposed to external consumers
- GraphQL API resolvers accepting external input
- Webhook receivers processing external events
- Public SDK interfaces and client libraries
- API gateway request/response models

Out of scope:
- Internal service-to-service communication within trusted boundaries
- Database models and ORM entities (separate validation layer)
- Background job processors (unless triggered by external events)
- Administrative or internal-only endpoints with separate authentication

Exceptions:
- EXC-001: Legacy API endpoints in maintenance mode with documented deprecation timeline
- EXC-002: Performance-critical endpoints where validation overhead is measured and documented as unacceptable

## Rationale

- Pattern detected across 75 files with 92.33% confidence indicates this is an established, mature practice in the codebase
- Type checking and validation frameworks (Pydantic, mypy) provide compile-time and runtime guarantees that prevent entire classes of bugs from reaching production
- Domain validation at API boundaries implements the 'fail fast' principle, providing immediate feedback to consumers and preventing invalid state propagation
- Consistent validation patterns reduce cognitive load for developers, improve code maintainability, and enable automated testing and documentation generation

## Consequences

Positive:
- Improved API reliability and data quality through early detection of invalid inputs
- Better developer experience for API consumers with clear, actionable error messages
- Reduced security vulnerabilities by preventing injection attacks and malformed data processing
- Enhanced maintainability through self-documenting code with explicit type annotations and validation rules
- Automated API documentation generation from typed models

Negative:
- Initial development overhead to define comprehensive validation rules for all API models
- Potential performance impact from validation processing on high-throughput endpoints
- Learning curve for developers unfamiliar with validation frameworks and type systems
- Increased complexity in error handling and response formatting logic

## Alternatives

- Manual validation using conditional statements within endpoint handlers (rejected)
  Rejected because: Leads to inconsistent validation logic, poor maintainability, and scattered validation rules across the codebase. Does not provide type safety or automated documentation.
  When valid: Never recommended for new development; only acceptable in legacy code scheduled for refactoring
- Schema validation using JSON Schema or OpenAPI specifications only (rejected)
  Rejected because: Separates validation logic from code, making it harder to maintain consistency. Lacks compile-time type checking and IDE support. Does not integrate well with Python type system.
  When valid: May be used as supplementary documentation but not as primary validation mechanism
- Hybrid approach with lightweight validation at API boundary and comprehensive validation in business logic layer (deferred)
  When valid: Could be considered for microservices architectures where business logic is separated from API layer, but requires clear ownership boundaries

## Risks

- Performance degradation on high-throughput endpoints due to validation overhead
  Mitigation: Profile validation performance, implement caching for compiled validators, consider async validation for non-blocking operations
  Owner: API Platform Team
- Breaking changes to existing API consumers if validation is added retroactively to legacy endpoints
  Mitigation: Implement validation in opt-in mode initially, provide migration period with warnings, version APIs appropriately
  Owner: API Architecture Team
- Overly strict validation rules may reject valid edge cases, frustrating legitimate API consumers
  Mitigation: Gather requirements from API consumers, implement comprehensive test suites with edge cases, provide clear exception request process
  Owner: Product Engineering Team

## Implementation Notes

- Use Pydantic v2 or similar validation framework with proven performance characteristics and active maintenance
- Implement custom error handlers to transform validation errors into consistent API error response format (e.g., RFC 7807 Problem Details)
- Create base model classes with common validation patterns to promote reuse and consistency across API models
- Configure mypy with strict mode and integrate into pre-commit hooks and CI pipeline to catch type errors early
- Document validation rules in API documentation using OpenAPI/Swagger annotations generated from model definitions
- Implement monitoring and alerting for validation failure rates to detect potential API abuse or consumer integration issues

## Continuation Context


Verify commands:
- grep -r 'class.*BaseModel' --include='*.py' | wc -l  # Count Pydantic models
- mypy --strict --config-file mypy.ini . 2>&1 | grep -c 'error'  # Check for type errors
- grep -r '@validator\|@field_validator' --include='*.py' | wc -l  # Count custom validators
- pytest tests/test_api_validation.py -v --cov=api --cov-report=term-missing  # Run validation tests

Accept when:
- All public API endpoint handlers accept only strongly-typed model instances, not raw dictionaries or Any types
- Mypy static type checking passes with zero errors in strict mode for all API modules
- API validation test suite achieves >90% code coverage with tests for valid inputs, invalid inputs, and edge cases
- API documentation is automatically generated from model type annotations and validation rules

## Enforcement

- Verified by: Automated mypy type checking in CI/CD pipeline with strict mode enabled
- Verified by: Pre-commit hooks running type checkers and validation framework linters
- Verified by: Code review checklist requiring validation framework usage for all new API endpoints
- Verified by: Automated API contract testing validating request/response schemas
- Violation handling: CI/CD pipeline fails if mypy reports type errors in API modules
- Violation handling: Pull requests blocked until code review approval confirms validation requirements met
- Violation handling: Runtime monitoring alerts on unexpected validation failure rates indicating missing validation
- Violation handling: Quarterly architecture reviews audit API endpoints for compliance with validation standards
- Exception process: Submit exception request to API Architecture Team with justification and alternative controls
- Exception process: Document exception in ADR exceptions log with approval signatures and expiration date
- Exception process: Implement compensating controls (e.g., additional monitoring, manual review) for approved exceptions
- Exception process: Review all active exceptions quarterly to determine if they can be resolved