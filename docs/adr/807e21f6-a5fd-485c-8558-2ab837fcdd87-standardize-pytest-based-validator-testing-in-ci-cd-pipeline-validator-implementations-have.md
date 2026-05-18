# Standardize Pytest-Based Validator Testing in CI/CD Pipeline: Validator Implementations Have

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The pydantic-core project requires comprehensive validation testing across multiple validator types (model_init, bool, arguments, misc) to ensure data validation correctness
- Test files follow a consistent pattern of pytest-based test organization with external client boundary testing, indicating a standardized CI/CD testing approach
- The facet 'boundaries.external_clients' suggests these tests validate behavior at system boundaries where external inputs are processed
- Multiple test files (4 detected) share the same structural pattern with high confidence (90.92%), indicating an established testing convention
- CI/CD pipelines need reliable, repeatable test execution to validate validator behavior before deployment

## Problem Statement

Without a standardized approach to validator testing in the CI/CD pipeline, there is risk of inconsistent test coverage, unpredictable validation behavior at external boundaries, and difficulty maintaining test quality as the validator library evolves. A unified testing pattern ensures all validators are tested consistently and reliably before integration.

## Decision

1. MUST: All validator implementations MUST have corresponding pytest-based test files in the tests/validators/ directory

## Policy Block

- MUST All validator implementations MUST have corresponding pytest-based test files in the tests/validators/ directory

In scope:
- All validator implementations in pydantic-core
- Test files under tests/ and tests/validators/ directories
- CI/CD pipeline test execution stages
- External client boundary validation logic

Out of scope:
- Integration tests with external systems
- Performance benchmarking tests
- Documentation examples (unless they serve as executable tests)
- Manual testing procedures

## Rationale

- The pattern signature (30aa88702c69df330b02aeeb6315df97) appears consistently across 4 test files with 90.92% confidence, indicating a deliberate and established testing pattern
- Validator testing at external boundaries is critical for data validation libraries like pydantic-core, where incorrect validation can lead to security vulnerabilities or data corruption
- Pytest provides a mature, widely-adopted testing framework with excellent CI/CD integration, making it the natural choice for Python validation testing
- Standardizing test structure reduces cognitive load for contributors and ensures consistent quality across the validator test suite

## Consequences

Positive:
- Consistent test coverage across all validators ensures predictable validation behavior
- CI/CD pipeline can reliably catch validator regressions before they reach production
- New contributors can easily understand and follow established testing patterns
- Boundary testing at external client interfaces reduces risk of validation bypass or unexpected behavior

Negative:
- Requires maintaining pytest infrastructure and keeping test dependencies up to date
- May increase CI/CD execution time as test suite grows with new validators
- Strict testing requirements may slow down rapid prototyping of new validators
- Test maintenance overhead increases proportionally with validator complexity

## Alternatives

- Use unittest framework instead of pytest for validator testing (rejected)
  Rejected because: Pytest offers superior parametrization, fixture management, and CI/CD integration compared to unittest, and is already the established pattern in the codebase
  When valid: For projects with existing unittest infrastructure or strict standard library requirements
- Implement property-based testing with Hypothesis for all validators (deferred)
  Rejected because: While property-based testing provides excellent coverage, it may be introduced incrementally alongside pytest tests rather than as a replacement
  When valid: For validators with complex input spaces where exhaustive example-based testing is impractical
- Manual testing only without automated CI/CD test execution (rejected)
  Rejected because: Manual testing is error-prone, not scalable, and provides no regression protection in CI/CD pipelines
  When valid: Never valid for production validation libraries

## Risks

- Test suite execution time may become prohibitive as validator count grows, slowing CI/CD feedback loops
  Mitigation: Implement test parallelization, selective test execution based on changed files, and regular performance profiling of slow tests
  Owner: CI/CD team
- Inconsistent test quality if contributors do not follow boundary testing guidelines
  Mitigation: Implement automated test coverage checks, code review checklists, and provide test templates for new validators
  Owner: Engineering team
- Pytest version updates may introduce breaking changes affecting test execution
  Mitigation: Pin pytest versions in requirements, test upgrades in isolated environments, and maintain compatibility testing in CI/CD
  Owner: DevOps team

## Implementation Notes

- Use pytest fixtures to share common test setup across validator test files, reducing duplication
- Organize tests by validator type in the tests/validators/ directory structure for clear navigation
- Include both positive test cases (valid inputs) and negative test cases (invalid inputs with expected errors) for comprehensive coverage
- Configure pytest in CI/CD to fail fast on first error during development, but run all tests in pre-merge validation
- Document expected test patterns in CONTRIBUTING.md to guide new contributors

## Continuation Context


Verify commands:
- find tests/validators -name 'test_*.py' | wc -l | grep -v '^0$'
- pytest tests/validators/ --collect-only --quiet | grep -c 'test session starts'
- grep -r 'import pytest' tests/validators/ | wc -l | grep -v '^0$'

Accept when:
- All validator test files are discoverable by pytest in tests/validators/ directory
- CI/CD pipeline successfully executes pytest and reports test results
- Test files contain boundary testing for external client inputs as evidenced by test function names or assertions

## Enforcement

- Verified by: CI/CD pipeline pytest execution with mandatory pass requirement before merge
- Verified by: Code review checklist verification that new validators include corresponding test files
- Verified by: Automated coverage reports showing test coverage for validator modules
- Violation handling: Pull requests without validator tests are blocked from merging by CI/CD checks
- Violation handling: Coverage drops below threshold trigger automated notifications to PR authors
- Violation handling: Quarterly audit of validator test coverage with remediation plans for gaps
- Exception process: Exceptions require written justification in PR description explaining why tests cannot be provided
- Exception process: Technical lead approval required for any validator merged without complete test coverage
- Exception process: Exception tracking in project documentation with remediation timeline