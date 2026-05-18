# Enforce Type Safety and Validation in Public API Contracts: Public Models Include

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all public API contracts, external integrations, and type validation implementations. All code that exposes public APIs or validates external data MUST comply with these rules.

## Context

- The codebase extensively uses Pydantic for runtime type validation and mypy for static type checking across 140 files, indicating a strong commitment to type safety in public-facing APIs
- Evidence shows comprehensive testing of type validation, error handling, JSON serialization, and timezone handling, suggesting APIs must handle diverse external inputs reliably
- The presence of mypy plugin configurations and strict type checking modules indicates enforcement of type contracts at both development and runtime
- Root models, class validators, and structural pattern matching are used to define and enforce API contracts with external consumers
- The pattern appears in core validation modules (pydantic/mypy.py, pydantic/root_model.py, pydantic/v1/main.py) and extensive test suites, demonstrating production-grade type safety requirements

## Problem Statement

Public APIs exposed to external consumers require robust type safety and validation to prevent runtime errors, data corruption, and security vulnerabilities. Without enforced type contracts, APIs become fragile, difficult to maintain, and prone to breaking changes that impact downstream consumers. The challenge is to establish consistent validation patterns that work across both static analysis (mypy) and runtime validation (Pydantic) while maintaining developer productivity and clear error messaging.

## Decision

1. MUST: All public API models MUST include comprehensive test coverage for type validation, including edge cases, invalid inputs, and boundary conditions

## Policy Block

- MUST All public API models MUST include comprehensive test coverage for type validation, including edge cases, invalid inputs, and boundary conditions

In scope:
- All REST API endpoints exposed to external consumers
- GraphQL schemas and resolvers accessible outside the organization
- SDK and library public interfaces
- Webhook handlers receiving data from third-party services
- Data ingestion pipelines processing external data sources
- Configuration files and environment variables affecting public API behavior

Out of scope:
- Internal microservice communication within trusted network boundaries (unless handling sensitive data)
- Database models and ORM schemas (unless directly exposed via API)
- Private utility functions not exposed in public interfaces
- Development and testing mock objects
- Legacy code scheduled for deprecation within 6 months

Exceptions:
- EXC-001: Performance-critical internal APIs where validation overhead exceeds 10% of request latency and all consumers are trusted internal services
- EXC-002: Gradual migration of legacy APIs where immediate strict validation would break existing consumers

## Rationale

- Pattern detected across 140 files with 92.02% confidence indicates this is a well-established, production-proven approach to API type safety
- Pydantic provides runtime validation that catches errors at the API boundary before they propagate into business logic, reducing debugging time and improving system reliability
- Mypy static type checking catches type errors during development, enabling faster feedback loops and preventing entire classes of runtime errors
- Comprehensive test coverage of validation logic (evidenced by test_typing.py, test_errors.py, test_json.py) demonstrates that type safety is a critical quality attribute requiring continuous verification

## Consequences

Positive:
- Reduced runtime errors and improved API reliability through early detection of type mismatches and invalid data
- Better developer experience with IDE autocomplete, inline documentation, and compile-time error detection
- Clearer API contracts that serve as living documentation for external consumers
- Easier refactoring and maintenance as type system prevents breaking changes from propagating silently
- Improved security posture by validating and sanitizing all external inputs at API boundaries

Negative:
- Initial development overhead to define comprehensive Pydantic models and type annotations
- Potential performance impact from runtime validation, especially for high-throughput APIs
- Learning curve for developers unfamiliar with advanced type systems and validation frameworks
- Increased complexity in handling gradual typing and migration of legacy untyped code
- Risk of overly strict validation rejecting valid edge cases if schemas are not carefully designed

## Alternatives

- Use runtime validation only (Pydantic) without static type checking (mypy) (rejected)
  Rejected because: Static type checking catches errors during development before code reaches production, providing faster feedback and preventing entire classes of errors. Runtime-only validation misses opportunities for early detection.
  When valid: May be acceptable for rapid prototyping or scripts with short lifespan
- Use static type checking only (mypy) without runtime validation (rejected)
  Rejected because: Static typing cannot validate external inputs at runtime. APIs must validate untrusted data from external sources, which requires runtime validation regardless of static type correctness.
  When valid: Only valid for internal functions with fully trusted inputs from type-checked code
- Use JSON Schema validation instead of Pydantic models (rejected)
  Rejected because: JSON Schema lacks Python type integration, requires separate schema definitions, and doesn't provide the same level of IDE support and type inference. Pydantic combines schema validation with Python type system.
  When valid: Valid when API contracts must be shared across multiple languages and JSON Schema is the common standard

## Risks

- Performance degradation in high-throughput APIs due to validation overhead
  Mitigation: Profile validation performance, use Pydantic's compiled mode, implement caching for repeated validations, and consider validation sampling for trusted sources
  Owner: API Platform Team
- Breaking changes when tightening validation on existing APIs with loose contracts
  Mitigation: Implement gradual rollout with monitoring, provide deprecation warnings, version APIs appropriately, and communicate changes to consumers with sufficient lead time
  Owner: API Product Team
- Developer resistance due to perceived complexity and development friction
  Mitigation: Provide comprehensive documentation, training sessions, reusable model templates, and clear examples. Demonstrate value through reduced production incidents.
  Owner: Engineering Leadership

## Implementation Notes

- Start by identifying all public API endpoints and creating an inventory of validation coverage gaps
- Use Pydantic V2 for new implementations to benefit from performance improvements via Rust core (pydantic-core)
- Configure mypy with strict mode in CI/CD pipeline and gradually increase strictness for existing code using per-module configuration
- Create shared base models and validators for common patterns (pagination, timestamps, IDs) to ensure consistency across APIs
- Implement comprehensive error handling that translates validation errors into appropriate HTTP status codes (400 Bad Request) with detailed error messages
- Document all public models with docstrings and examples that are automatically included in API documentation

## Continuation Context


Verify commands:
- grep -r "class.*BaseModel" --include="*.py" | wc -l  # Count Pydantic models
- mypy --strict --config-file mypy.ini . 2>&1 | grep -E "(error|Success)"  # Verify static type checking
- pytest tests/test_typing.py tests/test_errors.py -v  # Run type validation tests
- grep -r "@validator\|@field_validator" --include="*.py" | wc -l  # Count custom validators

Accept when:
- All public API endpoints have corresponding Pydantic models with 100% field coverage
- Mypy strict mode passes with zero errors for all public API modules
- Test coverage for validation logic exceeds 90% with tests for invalid inputs, edge cases, and error messages
- CI/CD pipeline includes automated validation checks that fail builds on type errors or missing validation

## Enforcement

- Verified by: Automated CI/CD pipeline checks running mypy in strict mode on all commits
- Verified by: Pre-commit hooks that run type checking locally before code is pushed
- Verified by: Code review checklist requiring validation coverage for all new API endpoints
- Verified by: Automated test coverage reports flagging modules below 90% validation test coverage
- Violation handling: CI/CD pipeline fails and blocks merge if mypy strict mode reports type errors in public API modules
- Violation handling: Pull requests without Pydantic models for new API endpoints are automatically flagged for review
- Violation handling: Production monitoring alerts on validation errors exceeding baseline thresholds
- Violation handling: Quarterly architecture reviews identify and prioritize remediation of validation gaps
- Exception process: Submit exception request to architecture review board with justification, risk assessment, and mitigation plan
- Exception process: Document exception in ADR addendum with approval signatures and expiration date
- Exception process: Create technical debt ticket with timeline for bringing code into compliance
- Exception process: Review all active exceptions quarterly and require re-justification or remediation