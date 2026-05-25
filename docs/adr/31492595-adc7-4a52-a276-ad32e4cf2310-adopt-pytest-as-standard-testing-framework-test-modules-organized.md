# Adopt pytest as Standard Testing Framework: Test Modules Organized

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The project contains extensive test suites across multiple modules including validators, serializers, and core functionality testing
- Test files follow a consistent naming convention (test_*.py) and are organized in dedicated test directories (tests/, pydantic-core/tests/)
- The codebase demonstrates a mature testing strategy with specialized test modules for different components (generics, dataclasses, bytes, dates, nullable types, etc.)
- Testing patterns indicate the need for a robust, feature-rich testing framework that supports parametrization, fixtures, and clear assertion reporting
- The project requires a testing framework that can handle complex validation scenarios, serialization testing, and type checking

## Problem Statement

The project needs a standardized testing framework that can support comprehensive test coverage across validators, serializers, and core functionality while providing clear test organization, powerful assertion capabilities, and maintainable test code. Without a consistent testing framework, test quality, readability, and maintainability would suffer, making it difficult to ensure reliability across the codebase.

## Decision

1. SHOULD: Test modules SHOULD be organized by component or feature area (validators, serializers, etc.)

## Policy Block

- SHOULD Test modules SHOULD be organized by component or feature area (validators, serializers, etc.)

In scope:
- All Python test files in the project
- Unit tests for validators and serializers
- Integration tests for core functionality
- Test utilities and helper modules
- Continuous integration test execution

Out of scope:
- Documentation examples that are not formal tests
- Performance benchmarking scripts that don't use pytest
- One-off debugging scripts
- Third-party test suites in dependencies

Exceptions:
- EX-001: Legacy test files from acquired codebases during migration period

## Rationale

- pytest is the de facto standard testing framework in the Python ecosystem, providing superior features compared to unittest including better assertion introspection, fixture management, and plugin ecosystem
- The detected pattern shows consistent usage across 10 test files with 92.11% confidence, indicating this is an established practice in the codebase
- pytest's parametrization capabilities are essential for testing validators and serializers with multiple input scenarios efficiently
- The framework's clear output and failure reporting significantly improves developer productivity when debugging test failures

## Consequences

Positive:
- Consistent testing approach across the entire codebase improves maintainability and reduces cognitive load for developers
- pytest's rich plugin ecosystem enables easy integration of coverage tools, performance profiling, and other testing enhancements
- Better assertion introspection provides clearer failure messages, reducing debugging time
- Fixture system promotes code reuse and cleaner test organization compared to class-based setup methods

Negative:
- Developers unfamiliar with pytest will need to learn framework-specific concepts like fixtures and parametrization
- Migration of any existing unittest-based tests requires effort and may introduce temporary inconsistency
- pytest's magic behavior (auto-discovery, fixture injection) can be confusing for newcomers
- Additional dependency in the project's test requirements

## Alternatives

- Use Python's built-in unittest framework (rejected)
  Rejected because: unittest requires more boilerplate code, has less intuitive assertion methods, and lacks pytest's advanced features like parametrization and powerful fixtures
  When valid: For projects with strict zero-dependency requirements or when maintaining legacy codebases
- Use nose2 testing framework (rejected)
  Rejected because: nose2 has limited active development and smaller community compared to pytest, and offers fewer advantages over pytest
  When valid: For projects already heavily invested in nose/nose2 with extensive custom plugins
- Mix multiple testing frameworks based on test type (rejected)
  Rejected because: Multiple frameworks increase complexity, create inconsistent developer experience, and complicate CI/CD pipeline configuration
  When valid: Never recommended; consistency is critical for maintainability

## Risks

- pytest version updates may introduce breaking changes in test behavior or plugin compatibility
  Mitigation: Pin pytest version in requirements, test upgrades in isolated environment before rolling out, maintain changelog of pytest-related changes
  Owner: Engineering team
- Over-reliance on pytest-specific features may make tests difficult to port if framework change is ever needed
  Mitigation: Keep test logic simple and focused, avoid excessive use of advanced pytest magic, document any complex fixture dependencies
  Owner: Engineering team
- Inconsistent pytest usage patterns across team members may lead to fragmented test styles
  Mitigation: Establish pytest best practices documentation, conduct code reviews focusing on test quality, provide pytest training for new team members
  Owner: Engineering team

## Implementation Notes

- Install pytest in development dependencies with a pinned version for reproducibility
- Configure pytest.ini or pyproject.toml with project-specific settings (test paths, markers, plugins)
- Create shared fixtures in conftest.py files at appropriate directory levels for reusable test setup
- Use pytest markers to categorize tests (unit, integration, slow) for selective test execution
- Integrate pytest with CI/CD pipeline using pytest-cov for coverage reporting and appropriate exit codes for build failures

## Continuation Context


Verify commands:
- grep -r "^import pytest" tests/ pydantic-core/tests/ | wc -l
- find tests/ pydantic-core/tests/ -name 'test_*.py' -type f | wc -l
- pytest --collect-only -q 2>&1 | grep -E '^[0-9]+ tests? collected'

Accept when:
- All test files in tests/ and pydantic-core/tests/ directories follow test_*.py naming convention
- pytest successfully discovers and collects all test functions without errors
- No unittest.TestCase class-based tests exist in the codebase (or documented exceptions only)

## Enforcement

- Verified by: Automated CI pipeline runs pytest and fails build if tests don't pass
- Verified by: Code review checklist includes verification of pytest usage in new test files
- Verified by: Static analysis tools check for test file naming conventions
- Verified by: Pre-commit hooks validate test file structure
- Violation handling: Pull requests with non-pytest test files are rejected during code review
- Violation handling: CI pipeline fails if test files don't follow naming conventions
- Violation handling: Automated comments on PRs highlight test framework violations
- Violation handling: Monthly audit reports identify any non-compliant test files for remediation
- Exception process: Developer submits exception request to tech lead with justification
- Exception process: Tech lead reviews and approves/rejects based on valid use case
- Exception process: Approved exceptions must be documented in test file header with ticket reference
- Exception process: Exceptions are reviewed quarterly for potential removal