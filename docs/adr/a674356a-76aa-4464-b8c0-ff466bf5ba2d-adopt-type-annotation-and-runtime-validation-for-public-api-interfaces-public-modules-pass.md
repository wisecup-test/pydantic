# Adopt Type Annotation and Runtime Validation for Public API Interfaces: Public Modules Pass

Status: proposed
Date: 2025-01-10
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all public and external API interfaces. All code that exposes public APIs must comply with the type annotation and validation requirements specified herein.

## Context

- The codebase demonstrates extensive use of type annotations, mypy plugin integration, and runtime validation frameworks (Pydantic) across 140 files with 92.02% confidence, indicating a systematic approach to API design
- Evidence from test files (test_typing.py, test_tzinfo.py, test_errors.py, test_json.py) and core modules (mypy.py, root_model.py, main.py) shows deliberate investment in type safety and validation infrastructure
- The presence of mypy plugin configurations, strict equality checks, and structural pattern matching tests indicates enforcement of type correctness at both static analysis and runtime levels
- Legacy v1 modules (v1/class_validators.py, v1/main.py) coexist with modern implementations, suggesting an evolutionary migration toward stricter type safety standards
- The pattern spans core library code, test suites, and plugin infrastructure, demonstrating that type safety is a cross-cutting architectural concern rather than a localized implementation detail

## Problem Statement

Public and external APIs require robust contracts to prevent integration failures, runtime errors, and breaking changes. Without comprehensive type annotations and runtime validation, API consumers face unpredictable behavior, difficult debugging, and fragile integrations. The challenge is establishing a consistent, enforceable standard for API interface definitions that provides both static type checking benefits and runtime safety guarantees.

## Decision

1. MUST: All public API modules MUST pass mypy strict mode type checking without errors or suppressions

## Policy Block

- MUST All public API modules MUST pass mypy strict mode type checking without errors or suppressions

In scope:
- All functions, methods, and classes exposed in public module __all__ declarations
- All API endpoints, request/response models, and data transfer objects
- All configuration classes and settings objects used by external consumers
- All exception classes and error types that may be raised to API consumers
- All callback interfaces and plugin extension points

Out of scope:
- Private module internals prefixed with underscore (_) that are not part of the public API
- Test utilities and fixtures used exclusively within test suites
- Temporary migration code marked for deprecation
- Third-party library integrations where type annotations are not available upstream

Exceptions:
- EXC-001: Legacy v1 API modules that are explicitly marked as deprecated and scheduled for removal
- EXC-002: Performance-critical hot paths where runtime validation overhead is measured and documented as unacceptable

## Rationale

- The pattern detection identified 140 files with 92.02% confidence demonstrating systematic adoption of type annotations and validation, indicating this is an established architectural standard rather than an experimental approach
- Evidence from mypy plugin integration (mypy.py) and strict configuration files shows organizational commitment to static type checking as a quality gate
- The presence of comprehensive test coverage for typing behavior (test_typing.py, test_errors.py, test_json.py) demonstrates that type safety is actively tested and maintained as a first-class concern
- Runtime validation frameworks like Pydantic provide both developer experience benefits (IDE autocomplete, early error detection) and production safety (input sanitization, clear error messages)

## Consequences

Positive:
- Improved API reliability and reduced runtime errors through early detection of type mismatches at both development time (mypy) and runtime (validation)
- Enhanced developer experience with better IDE support, autocomplete, and inline documentation derived from type hints
- Clearer API contracts that serve as executable documentation, reducing integration confusion and support burden
- Easier refactoring and maintenance with confidence that type-related breaking changes will be caught by static analysis

Negative:
- Increased initial development time to write comprehensive type annotations and validation logic for all public APIs
- Runtime performance overhead from validation checks, particularly for high-throughput API endpoints
- Learning curve for developers unfamiliar with advanced typing features (generics, protocols, type variables)
- Potential maintenance burden when updating type annotations across large codebases during API evolution

## Alternatives

- Use runtime validation only without static type annotations (rejected)
  Rejected because: Loses development-time benefits of IDE support and early error detection; increases debugging time and reduces code maintainability
  When valid: May be acceptable for rapid prototyping or internal scripts with short lifespans
- Use static type annotations only without runtime validation (rejected)
  Rejected because: Provides no protection against invalid data from external sources (user input, API calls, file parsing); type annotations are not enforced at runtime in Python
  When valid: Acceptable for internal modules with trusted data sources and comprehensive unit test coverage
- Adopt gradual typing with optional annotations for non-critical paths (deferred)
  Rejected because: Not rejected but deferred for internal implementation details; public APIs require strict enforcement
  When valid: Valid for internal helper functions and private modules that are not exposed through public interfaces

## Risks

- Performance degradation in high-throughput APIs due to runtime validation overhead
  Mitigation: Profile validation performance, implement caching for repeated validations, provide opt-out mechanisms for trusted internal calls with clear documentation
  Owner: API Platform Team
- Breaking changes when tightening type annotations on existing APIs
  Mitigation: Use deprecation warnings, provide migration guides, version APIs appropriately, and use mypy's --warn-unused-ignores to track suppression usage
  Owner: Engineering Team
- Inconsistent adoption across teams leading to fragmented API quality
  Mitigation: Enforce through CI/CD pipeline checks, provide templates and examples, conduct code review training, and automate compliance verification
  Owner: Developer Experience Team

## Implementation Notes

- Start by adding type annotations to function signatures, then progressively add validation decorators or Pydantic models for complex data structures
- Configure mypy in strict mode with --strict flag or enable individual strict options (--disallow-untyped-defs, --disallow-any-generics, --warn-return-any)
- Use Pydantic's BaseModel for data classes and validation, leveraging Field() for constraints and custom validators for complex business logic
- Create reusable type aliases and generic types for common patterns (e.g., ID types, pagination parameters) to maintain consistency across APIs
- Document type annotation patterns in team style guides with examples of correct usage for common scenarios (Optional vs Union[X, None], Sequence vs List, etc.)

## Continuation Context


Verify commands:
- mypy --strict --config-file mypy.ini $(find . -name '*.py' -path '*/api/*' -o -path '*/public/*')
- grep -r 'def ' --include='*.py' api/ | grep -v '\->' | grep -v '^\s*#' || echo 'All public functions have return type annotations'
- python -m pytest tests/test_typing.py tests/test_validation.py -v --tb=short

Accept when:
- All public API modules pass mypy strict mode checking with zero errors and zero type: ignore comments
- All public API functions have complete type annotations for parameters and return values verified by automated linting
- Runtime validation tests demonstrate that invalid inputs are rejected with clear error messages for all public API endpoints

## Enforcement

- Verified by: CI/CD pipeline runs mypy in strict mode on all public API modules and fails builds on type checking errors
- Verified by: Pre-commit hooks validate type annotation completeness using automated linting tools
- Verified by: Code review checklist includes verification of type annotations and runtime validation for all API changes
- Verified by: Automated test suite includes type-focused tests that verify validation behavior and error handling
- Violation handling: CI/CD pipeline blocks merging of pull requests that fail mypy strict mode checks
- Violation handling: Code review process requires explicit justification and architectural approval for any type: ignore suppressions
- Violation handling: Quarterly audits identify modules with incomplete type coverage and create remediation tickets
- Violation handling: New API endpoints without proper type annotations are rejected during design review phase
- Exception process: Submit exception request to architecture review board with detailed justification, impact analysis, and mitigation plan
- Exception process: Provide evidence of technical necessity (e.g., performance benchmarks, third-party library limitations)
- Exception process: Document approved exceptions in ADR amendments with expiration dates and review schedules
- Exception process: Track exceptions in technical debt register with assigned owners and remediation timelines