# Adopt pytest as Standard Testing Framework for Python Projects: Complex Test Scenarios

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The project contains 47 test files following a consistent pytest-based testing pattern with 91.15% confidence
- Test files are organized under a dedicated tests/ directory with subdirectories for validators, serializers, and other functional areas
- The codebase demonstrates mature testing practices including parametrized tests, fixtures, and comprehensive validator testing
- Testing infrastructure is critical for CI/CD pipelines to ensure code quality and prevent regressions in production deployments
- The pattern shows consistent use of pytest conventions across multiple test modules including test_typing.py, test_build.py, test_strict.py, and numerous validator tests

## Problem Statement

Python projects require a standardized, feature-rich testing framework that integrates seamlessly with CI/CD pipelines, supports advanced testing patterns like parametrization and fixtures, and provides clear test output for debugging. Without a consistent testing framework, teams face fragmented tooling, inconsistent test execution, and difficulty maintaining test quality across the codebase.

## Decision

1. SHOULD: Complex test scenarios SHOULD use pytest.mark.parametrize for data-driven testing to reduce code duplication

## Policy Block

- SHOULD Complex test scenarios SHOULD use pytest.mark.parametrize for data-driven testing to reduce code duplication

In scope:
- All Python test files in the tests/ directory
- Unit tests, integration tests, and functional tests for Python code
- CI/CD pipeline test execution stages
- Local development test runs
- Pre-commit and pre-push test hooks

Out of scope:
- Non-Python test files (e.g., JavaScript, Go tests)
- Performance benchmarking tests that may use specialized frameworks
- End-to-end tests that may use different tooling (e.g., Selenium, Playwright)
- Documentation tests that may use doctest
- Legacy test files in migration period (temporary exemption)

Exceptions:
- EXC-001: Legacy test files exist that use unittest framework during migration period
- EXC-002: Specialized testing scenarios require alternative frameworks (e.g., hypothesis for property-based testing)

## Rationale

- pytest is the de facto standard testing framework in the Python ecosystem with widespread adoption, extensive plugin ecosystem, and superior developer experience compared to alternatives like unittest
- The detected pattern shows 47 files with 91.15% confidence, indicating strong organizational commitment and mature implementation of pytest-based testing practices
- pytest's fixture system, parametrization capabilities, and clear assertion introspection significantly improve test maintainability and debugging efficiency
- Standardizing on pytest enables consistent CI/CD integration, reduces onboarding time for new developers, and facilitates knowledge sharing across teams

## Consequences

Positive:
- Consistent testing approach across all Python projects reduces cognitive load and improves developer productivity
- pytest's rich plugin ecosystem enables easy integration of coverage reporting, parallel test execution, and other advanced features
- Clear and informative test output accelerates debugging and reduces time spent investigating test failures
- Strong community support and extensive documentation lower the barrier to writing high-quality tests
- Fixture-based dependency injection promotes test isolation and reusability

Negative:
- Teams familiar with unittest or other frameworks face a learning curve when adopting pytest conventions
- Existing test suites using alternative frameworks require migration effort and potential refactoring
- pytest's magic behavior (e.g., automatic fixture discovery) may be less explicit than traditional setup/teardown patterns
- Additional dependency in project requirements increases maintenance surface area
- Some advanced pytest features may introduce complexity for simple test scenarios

## Alternatives

- Use Python's built-in unittest framework as the standard testing framework (rejected)
  Rejected because: unittest requires more boilerplate code, lacks advanced features like parametrization and fixtures, and provides less informative assertion output compared to pytest. The detected pattern shows clear organizational preference for pytest.
  When valid: May be appropriate for projects with strict zero-dependency requirements or when maintaining legacy codebases that heavily use unittest
- Allow teams to choose their preferred testing framework on a per-project basis (rejected)
  Rejected because: Fragmented tooling increases maintenance burden, complicates CI/CD pipeline configuration, and hinders developer mobility across projects. Standardization provides greater long-term value.
  When valid: Only appropriate during transition periods or for experimental projects outside main product lines
- Adopt nose2 as a unittest-compatible test runner with enhanced features (rejected)
  Rejected because: nose2 has significantly smaller community adoption, less active development, and fewer plugins compared to pytest. The original nose framework is deprecated, raising concerns about long-term viability.
  When valid: Not recommended; pytest provides superior functionality and ecosystem support

## Risks

- Migration of large existing test suites from unittest to pytest may introduce regressions or require significant engineering effort
  Mitigation: Implement gradual migration strategy using pytest's unittest compatibility mode. Run both frameworks in parallel during transition. Prioritize high-value test modules for migration first.
  Owner: Engineering team leads
- Over-reliance on pytest-specific features may create vendor lock-in and complicate future framework changes
  Mitigation: Focus on standard pytest features with broad adoption. Document framework-specific patterns. Maintain clean separation between test logic and framework-specific code.
  Owner: Architecture review board
- Inconsistent pytest usage patterns across teams may emerge without clear guidelines and examples
  Mitigation: Establish pytest best practices documentation, provide example test templates, conduct training sessions, and implement code review guidelines for test quality.
  Owner: Developer experience team

## Implementation Notes

- Install pytest in project dependencies (requirements.txt or pyproject.toml) with version pinning for reproducibility
- Configure pytest.ini or pyproject.toml [tool.pytest.ini_options] section to define test discovery paths, markers, and output options
- Structure tests/ directory to mirror source code organization (e.g., tests/validators/ for src/validators/)
- Create conftest.py files at appropriate levels to define shared fixtures and configuration
- Integrate pytest into CI/CD pipelines with appropriate flags (e.g., -v for verbose output, --tb=short for concise tracebacks, --maxfail=1 for fail-fast)
- Use pytest-cov plugin for code coverage reporting and enforce minimum coverage thresholds in CI
- Document project-specific pytest conventions in CONTRIBUTING.md or testing guidelines

## Continuation Context


Verify commands:
- grep -r "^import pytest" tests/ || grep -r "^from pytest" tests/
- find tests/ -name 'test_*.py' -o -name '*_test.py' | head -5
- pytest --collect-only tests/ 2>&1 | grep -E '(test session starts|collected)'
- grep -E "pytest|pytest-cov" requirements.txt pyproject.toml setup.py setup.cfg 2>/dev/null || echo 'Check dependency files'

Accept when:
- pytest is listed as a dependency in project requirements files (requirements.txt, pyproject.toml, or setup.py)
- Test files follow pytest naming conventions (test_*.py) and are organized in tests/ directory
- pytest --collect-only successfully discovers test files without errors
- CI/CD pipeline configuration includes pytest execution commands
- At least 80% of test files import pytest or use pytest-specific features (fixtures, parametrize, marks)

## Enforcement

- Verified by: Automated CI/CD pipeline checks that execute pytest and verify test discovery
- Verified by: Code review process validates new test files follow pytest conventions
- Verified by: Static analysis tools check for pytest imports and naming patterns
- Verified by: Pre-commit hooks run pytest on modified test files
- Verified by: Periodic audits of tests/ directory structure and naming compliance
- Violation handling: CI/CD pipeline fails if pytest is not installed or test discovery fails
- Violation handling: Code review blocks merge requests that introduce non-compliant test files
- Violation handling: Automated linting tools flag test files that don't follow naming conventions
- Violation handling: Build warnings generated for tests/ files that don't import pytest
- Violation handling: Technical debt tickets created for legacy test files requiring migration
- Exception process: Submit exception request to tech lead with justification for alternative testing approach
- Exception process: Architecture review board evaluates exception requests for specialized testing scenarios
- Exception process: Approved exceptions documented in project README or TESTING.md with rationale and scope
- Exception process: Exception reviews conducted quarterly to assess continued validity
- Exception process: All exceptions require documented integration plan with primary pytest-based test suite