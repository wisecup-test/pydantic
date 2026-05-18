# Adopt Comprehensive Test Suite Organization for Core Library Validation: Test Suites Include

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The pydantic-core library requires extensive validation testing across multiple validator types, serializers, and type handling mechanisms to ensure correctness and reliability
- A structured test organization pattern emerged across 48 test files with 91.14% consistency, indicating a deliberate architectural approach to test coverage
- Tests are organized hierarchically by functional domain (validators, serializers, arguments) with specific test files for each validation type (float, complex, frozenset, none, etc.)
- The test suite includes both unit tests for individual validators and integration tests for complex scenarios like lax/strict mode handling and model root validation
- This pattern supports continuous integration by providing comprehensive, organized test coverage that can be executed efficiently in CI/CD pipelines

## Problem Statement

Core validation libraries require extensive test coverage across numerous edge cases, type combinations, and validation scenarios. Without a systematic test organization strategy, test suites become difficult to maintain, gaps in coverage emerge, and CI/CD execution becomes inefficient. The challenge is to establish a consistent, scalable test organization pattern that ensures comprehensive coverage while remaining maintainable and efficient for automated testing pipelines.

## Decision

1. MAY: Test suites MAY include integration tests that validate cross-cutting concerns like typing, docstrings, and build processes

## Policy Block

- MAY Test suites MAY include integration tests that validate cross-cutting concerns like typing, docstrings, and build processes

In scope:
- All test files within the pydantic-core/tests directory
- Validator tests covering primitive types (int, float, str, bool, none, complex)
- Validator tests covering collection types (list, set, frozenset, dict, tuple)
- Serializer tests for type conversion and output formatting
- Argument validation tests including positional, keyword, and variadic arguments
- Integration tests for typing, build processes, and documentation

Out of scope:
- Application-level tests that use pydantic-core as a dependency
- Performance benchmarking tests (unless specifically for CI/CD validation)
- Manual testing procedures and exploratory testing
- Third-party library tests that integrate with pydantic-core

Exceptions:
- EXC-001: Legacy test files may deviate from naming conventions if they predate this ADR and refactoring would introduce regression risk
- EXC-002: Experimental or prototype validators may have co-located tests during initial development phase

## Rationale

- The pattern was detected across 48 files with 91.14% confidence, indicating strong consistency and deliberate architectural choice rather than accidental convergence
- Hierarchical test organization by functional domain enables efficient test discovery, maintenance, and parallel execution in CI/CD pipelines
- Dedicated test files per validator type provide clear ownership boundaries and make it easy to identify coverage gaps
- The pattern supports both unit-level validation (individual validators) and integration-level validation (cross-cutting concerns like typing and build processes)
- Consistent test organization reduces cognitive load for contributors and makes it easier to add new validators following established patterns

## Consequences

Positive:
- Improved test maintainability through clear organizational structure and naming conventions
- Enhanced CI/CD pipeline efficiency through support for parallel test execution and targeted test selection
- Better test coverage visibility with clear mapping between validators and their corresponding test files
- Reduced onboarding time for new contributors who can quickly understand test organization patterns
- Easier identification of coverage gaps when validator types lack corresponding test files

Negative:
- Increased directory depth may make navigation more complex for small test suites
- Strict naming conventions may feel restrictive for tests that span multiple validator types
- Refactoring existing test suites to match this pattern requires significant effort
- May lead to test file proliferation with many small test files for simple validators

## Alternatives

- Flat test directory structure with all test files in a single directory (rejected)
  Rejected because: Does not scale well beyond 20-30 test files; makes it difficult to identify functional domains and creates namespace conflicts
  When valid: May be acceptable for very small libraries with fewer than 15 test files
- Co-locate tests with source code in the same directory structure (rejected)
  Rejected because: Mixes production code with test code, complicates packaging, and makes it harder to exclude tests from distribution artifacts
  When valid: Valid for projects that use pytest's conftest.py discovery and want tight coupling between code and tests
- Group tests by feature or user story rather than by technical component (rejected)
  Rejected because: Core validation libraries are component-focused rather than feature-focused; technical organization better matches the architecture
  When valid: More appropriate for application-level testing where user journeys are the primary concern

## Risks

- Test file proliferation may lead to excessive directory depth and navigation complexity
  Mitigation: Limit hierarchy to 3 levels maximum (tests/<domain>/<subdomain>/test_<type>.py); use clear naming conventions; provide documentation with directory structure overview
  Owner: Engineering team / Test infrastructure lead
- Rigid structure may not accommodate cross-cutting tests that span multiple validators or domains
  Mitigation: Allow integration test directory at root level for cross-cutting concerns; document exceptions for tests that don't fit the standard pattern
  Owner: Engineering team
- Migration of existing test suites may introduce regressions or break CI/CD pipelines
  Mitigation: Implement gradual migration strategy; maintain backward compatibility during transition; use CI/CD to validate test coverage remains constant
  Owner: DevOps team / Engineering lead

## Implementation Notes

- Start by creating the directory structure: tests/validators/, tests/serializers/, tests/arguments_v3/ as primary domains
- Use pytest's test discovery conventions: prefix test files with test_ and test functions with test_
- Implement pytest markers (@pytest.mark) to tag tests by category (unit, integration, slow) for selective CI/CD execution
- Create a tests/README.md documenting the organizational structure and conventions for contributors
- Set up CI/CD pipeline to run tests in parallel by directory to maximize execution speed
- Use code coverage tools to identify validators without corresponding test files and track coverage metrics per domain

## Continuation Context


Verify commands:
- find tests/ -type f -name 'test_*.py' | grep -E 'tests/(validators|serializers|arguments)/' | wc -l
- pytest tests/ --collect-only --quiet | grep -c '<Module'
- pytest tests/ -v --tb=short --maxfail=1

Accept when:
- All test files follow the naming convention test_<component>.py and are organized in appropriate domain subdirectories
- Test discovery finds all test files and pytest can execute the full suite without import errors
- CI/CD pipeline successfully executes tests with clear pass/fail status and coverage reports

## Enforcement

- Verified by: CI/CD pipeline test execution on every pull request
- Verified by: Code review checklist verifying new tests follow organizational structure
- Verified by: Automated linting rules checking test file naming conventions and directory placement
- Verified by: Coverage reports identifying validators without corresponding test files
- Violation handling: Pull requests with incorrectly organized tests receive automated comments with guidance
- Violation handling: CI/CD pipeline warnings for tests in non-standard locations (does not block merge)
- Violation handling: Quarterly audit of test organization with refactoring tasks created for violations
- Violation handling: New validator additions without corresponding tests block merge until tests are added
- Exception process: Developer documents exception rationale in pull request description
- Exception process: Tech lead or test infrastructure lead reviews and approves exception
- Exception process: Exception is documented in test file header with ADR reference and expiration date if temporary
- Exception process: Exceptions are tracked in a central registry and reviewed quarterly for potential removal