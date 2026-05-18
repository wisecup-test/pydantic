# Standardize Unit Testing Patterns for Public API Validation: Unit Tests Public

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all public/external API development and testing activities.

## Context

- The codebase contains 52 files exhibiting consistent unit testing patterns specifically focused on public API validation, with 92.21% confidence in pattern detection
- Public and external APIs require rigorous validation to ensure contract stability, backward compatibility, and correct behavior across diverse client integrations
- Test files demonstrate systematic coverage of typing, error handling, JSON serialization, validators, and structural pattern matching for API surface areas
- The pattern emerges from pydantic and pydantic-core test suites, which validate data validation and serialization APIs used by external consumers
- Consistent testing patterns across validators (dict, string, tagged_union, model, uuid, is_instance) indicate a deliberate architectural approach to API quality assurance

## Problem Statement

Public and external APIs require comprehensive, consistent unit testing to prevent breaking changes, ensure type safety, validate error handling, and maintain contract stability. Without standardized testing patterns, API quality becomes inconsistent, regression risks increase, and client integrations become fragile. The challenge is establishing mandatory testing practices that cover all critical API surface areas while remaining maintainable and enforceable.

## Decision

1. MUST: Unit tests for public APIs MUST cover type validation, error handling, and edge cases including null/empty inputs

## Policy Block

- MUST Unit tests for public APIs MUST cover type validation, error handling, and edge cases including null/empty inputs

In scope:
- All public API functions, methods, and classes exposed to external consumers
- Validator functions and validation logic accessible through public interfaces
- Data serialization/deserialization methods (JSON, dict conversion)
- Error types and error handling mechanisms in public APIs
- Type hints and typing utilities exposed as public API surface

Out of scope:
- Internal implementation details not exposed through public APIs
- Private helper functions and internal utilities
- Integration tests and end-to-end tests (covered by separate ADRs)
- Performance benchmarks and load testing
- Documentation examples (unless they serve as executable tests)

Exceptions:
- EXC-001: Deprecated API functions scheduled for removal in next major version
- EXC-002: Experimental APIs marked with explicit unstable/beta status

## Rationale

- Pattern detection identified 52 files with 92.21% confidence, demonstrating strong empirical evidence of this testing approach across the codebase
- Public APIs represent the contract between the system and external consumers; comprehensive unit testing prevents breaking changes and ensures backward compatibility
- Systematic testing of validators, error handling, and serialization reduces production defects and improves API reliability for downstream consumers
- The pydantic ecosystem's success depends on robust validation guarantees, making comprehensive unit testing a critical quality gate

## Consequences

Positive:
- Improved API stability and reduced breaking changes through comprehensive contract validation
- Earlier detection of regressions and type safety issues during development
- Increased confidence in refactoring and internal optimizations without affecting public behavior
- Better documentation through executable test examples that demonstrate correct API usage
- Reduced support burden from fewer API misuse issues and clearer error messages

Negative:
- Increased initial development time for new API features due to mandatory comprehensive test coverage
- Higher maintenance burden when API contracts evolve, requiring test updates across multiple files
- Potential for test brittleness if tests are too tightly coupled to implementation details
- Additional CI/CD execution time as test suite grows with API surface area

## Alternatives

- Rely primarily on integration tests and end-to-end tests for API validation (rejected)
  Rejected because: Integration tests are slower, harder to debug, and provide less precise failure localization compared to unit tests. They also cannot exhaustively cover edge cases and error conditions.
  When valid: May be used as complementary testing layer but not as replacement for unit tests
- Use contract testing frameworks (e.g., Pact) instead of traditional unit tests (rejected)
  Rejected because: Contract testing requires consumer-driven contracts and coordination with external teams. Unit tests provide faster feedback and are fully under internal control.
  When valid: Should be used in addition to unit tests for critical external integrations
- Generate tests automatically from API schemas or type definitions (deferred)
  Rejected because: While promising, automated test generation may miss business logic edge cases and requires mature tooling. Current manual approach provides better coverage.
  When valid: Should be revisited as test generation tools mature, potentially as supplement to manual tests

## Risks

- Test coverage may become incomplete as API surface area grows rapidly, creating gaps in validation
  Mitigation: Implement automated coverage tracking with minimum thresholds (e.g., 90% for public API modules) enforced in CI pipeline
  Owner: Engineering Team / QA Lead
- Tests may become tightly coupled to implementation details, making refactoring difficult
  Mitigation: Focus tests on public API contracts and observable behavior rather than internal implementation. Regular test review during code reviews.
  Owner: Engineering Team / Tech Lead
- Inconsistent test quality and patterns across different API modules as team grows
  Mitigation: Establish test templates and examples, conduct test-focused code reviews, and provide testing guidelines in developer documentation
  Owner: Engineering Team / Developer Experience Team

## Implementation Notes

- Organize test files to mirror the structure of the API module being tested (e.g., validators/test_dict.py tests validators/dict.py)
- Use descriptive test names that clearly indicate what contract or behavior is being validated (e.g., test_dict_validator_rejects_invalid_keys)
- Group related test cases using test classes or pytest markers to improve test organization and selective execution
- Include both positive test cases (valid inputs) and negative test cases (invalid inputs, edge cases, error conditions) for comprehensive coverage
- Leverage parametrized tests to efficiently cover multiple input variations without code duplication
- Document complex test scenarios with comments explaining the API contract being validated

## Continuation Context


Verify commands:
- pytest tests/ pydantic-core/tests/ -v --cov=. --cov-report=term-missing --cov-fail-under=90
- find tests/ pydantic-core/tests/ -name 'test_*.py' | wc -l
- grep -r "def test_" tests/ pydantic-core/tests/ | wc -l

Accept when:
- All public API modules have corresponding test files with minimum 90% code coverage
- Test suite executes successfully with all tests passing in CI pipeline
- Each public API function/class has at least one positive test case and one negative/error test case
- Test files follow established naming conventions (test_<module>.py) and organizational structure

## Enforcement

- Verified by: Automated code coverage analysis in CI pipeline with minimum threshold enforcement
- Verified by: Code review checklist requiring test coverage verification for all API changes
- Verified by: Pre-commit hooks validating test file existence for modified API modules
- Verified by: Periodic test quality audits reviewing test comprehensiveness and patterns
- Violation handling: CI pipeline fails if code coverage drops below 90% threshold for public API modules
- Violation handling: Pull requests without adequate test coverage are blocked from merging
- Violation handling: Violations are flagged in code review with required changes before approval
- Violation handling: Repeated violations trigger team discussion and potential process improvements
- Exception process: Developer submits exception request to Tech Lead with justification and risk assessment
- Exception process: Tech Lead reviews exception request considering impact on API stability and consumer risk
- Exception process: Approved exceptions are documented in ADR exceptions log with expiration date
- Exception process: Exceptions are reviewed quarterly and must be resolved or re-justified