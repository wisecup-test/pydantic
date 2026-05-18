# Adopt Pydantic V1 Compatibility Layer for Type Validation: Json Schema Generation

Status: proposed
Date: 2024-01-20
Deciders: Detection Pipeline (automated)

## Context

- The codebase contains Pydantic V1 compatibility imports through pydantic/v1/__init__.py, indicating a migration strategy from Pydantic V1 to V2
- Type validation and schema generation are critical components across 5 files including type adapters, JSON schema examples, and pipeline APIs
- The project uses Pydantic for runtime type checking, data validation, and JSON schema generation as evidenced by test files for type_adapter.py, json_schema_examples.py, and pipeline_api.py
- Pydantic-core validators (test_with_default.py) are being tested, suggesting deep integration with Pydantic's validation engine
- The pattern shows consistent usage across both main code (pydantic/v1) and test infrastructure, indicating this is a foundational architectural choice

## Problem Statement

The codebase needs a standardized approach to type validation, data serialization, and schema generation that supports both legacy Pydantic V1 code and modern Pydantic V2 features during a migration period, while maintaining type safety and runtime validation guarantees across the entire application.

## Decision

1. SHOULD: JSON schema generation SHOULD use Pydantic's built-in schema generation capabilities rather than custom implementations

## Policy Block

- SHOULD JSON schema generation SHOULD use Pydantic's built-in schema generation capabilities rather than custom implementations

In scope:
- All Python modules requiring runtime type validation
- Data serialization and deserialization logic
- API request/response validation
- Configuration file parsing and validation
- JSON schema generation for API documentation
- Type checking in test suites

Out of scope:
- Static type checking with mypy or pyright (complementary, not replaced)
- Simple type hints without runtime validation
- Performance-critical paths where validation overhead is prohibitive
- Third-party libraries with their own validation mechanisms

Exceptions:
- EXC-001: Performance profiling demonstrates validation overhead exceeds 10% of total execution time in critical paths
- EXC-002: Third-party library integration requires incompatible validation framework

## Rationale

- Pydantic provides industry-standard runtime type validation with excellent performance characteristics and wide ecosystem support
- The V1 compatibility layer (pydantic.v1) enables gradual migration from Pydantic V1 to V2, reducing migration risk and allowing incremental updates
- Evidence from 5 files with 89.66% confidence indicates this is an established pattern, not an experimental choice
- TypeAdapter and JSON schema generation capabilities provide flexible validation options beyond traditional model classes
- Integration with pydantic-core ensures validation logic is consistent with the underlying validation engine

## Consequences

Positive:
- Consistent validation behavior across the entire codebase with a single source of truth
- Automatic JSON schema generation reduces documentation burden and ensures API contracts match implementation
- Type safety at runtime catches errors that static type checkers cannot detect
- Gradual migration path from V1 to V2 reduces risk and allows teams to migrate at their own pace
- Rich ecosystem of Pydantic plugins and integrations (FastAPI, SQLModel, etc.) becomes available

Negative:
- Additional runtime overhead for validation compared to no validation (typically 5-15% in most cases)
- Learning curve for developers unfamiliar with Pydantic's model-based validation approach
- Migration complexity during V1 to V2 transition period requires careful namespace management
- Tight coupling to Pydantic means switching validation frameworks would require significant refactoring
- Validation errors may be verbose and require custom error handling for user-facing messages

## Alternatives

- Use Python's built-in dataclasses with manual validation logic (rejected)
  Rejected because: Lacks runtime validation, JSON schema generation, and requires significant boilerplate for complex validation rules. Does not provide the rich validation ecosystem that Pydantic offers.
  When valid: Appropriate for simple data containers without validation requirements
- Use marshmallow for serialization and validation (rejected)
  Rejected because: Marshmallow uses schema-based approach rather than type-based, resulting in more verbose code and less integration with modern Python type hints. Performance is generally slower than Pydantic.
  When valid: Valid for projects already heavily invested in marshmallow ecosystem
- Implement custom validation framework tailored to project needs (rejected)
  Rejected because: High development and maintenance cost, reinventing well-solved problems, and missing ecosystem integrations. Would require significant engineering effort to match Pydantic's feature set.
  When valid: Only when validation requirements are so unique that no existing framework can accommodate them

## Risks

- Pydantic V1 to V2 migration may introduce breaking changes in validation behavior or API surface
  Mitigation: Maintain comprehensive test coverage in tests/typechecking/ directory. Use V1 compatibility layer during transition. Migrate incrementally with thorough testing at each step.
  Owner: Engineering team leads
- Performance degradation in high-throughput scenarios due to validation overhead
  Mitigation: Profile validation performance in critical paths. Use Pydantic's performance optimization features (model_config, validation_alias). Consider validation caching for repeated operations. Document exception process for performance-critical code.
  Owner: Performance engineering team
- Developers may bypass validation by using unvalidated dictionaries or incorrect import paths
  Mitigation: Enforce through code review and linting rules. Add pre-commit hooks to detect direct pydantic imports in legacy code. Provide clear migration documentation and examples.
  Owner: Engineering team and code review process

## Implementation Notes

- Import Pydantic V1 APIs from pydantic.v1 namespace: 'from pydantic.v1 import BaseModel' for legacy code
- Use Pydantic V2 TypeAdapter for dynamic validation: 'TypeAdapter(MyType).validate_python(data)'
- Place type checking tests in tests/typechecking/ directory following existing patterns (type_adapter.py, json_schema_examples.py, pipeline_api.py)
- Test custom validators against pydantic-core validation engine as shown in pydantic-core/tests/validators/test_with_default.py
- Use model_config for Pydantic V2 configuration instead of Config class from V1
- Generate JSON schemas using model.model_json_schema() in V2 or model.schema() in V1 compatibility mode

## Continuation Context


Verify commands:
- grep -r 'from pydantic import' --include='*.py' . | grep -v 'from pydantic.v1 import' | grep -v 'from pydantic import' || echo 'V1 imports properly namespaced'
- find . -path '*/tests/typechecking/*.py' -type f | wc -l | awk '{if($1>0) print "Type checking tests found"; else exit 1}'
- python -c 'import pydantic; from pydantic.v1 import BaseModel; from pydantic import TypeAdapter; print("Pydantic imports successful")'

Accept when:
- All Pydantic V1 imports use the pydantic.v1 namespace, not direct pydantic imports
- Type checking test files exist in tests/typechecking/ directory covering TypeAdapter, JSON schema, and pipeline APIs
- Pydantic and pydantic.v1 modules can be imported successfully without conflicts
- Validator tests demonstrate compatibility with pydantic-core validation engine

## Enforcement

- Verified by: Pre-commit hooks checking for incorrect Pydantic import patterns
- Verified by: CI pipeline running type checking tests in tests/typechecking/ directory
- Verified by: Code review checklist requiring validation logic for data models
- Verified by: Automated linting rules detecting direct pydantic imports in legacy code paths
- Violation handling: CI build fails if type checking tests fail or incorrect import patterns detected
- Violation handling: Code review blocks merge if validation is missing from data models
- Violation handling: Linter warnings escalated to errors for direct pydantic imports outside allowed contexts
- Violation handling: Quarterly audit of validation coverage with remediation plans for gaps
- Exception process: Submit exception request to architecture review board with performance data or technical justification
- Exception process: Document exception in ADR supplement with approval signatures
- Exception process: Add inline comments explaining why validation is bypassed with ticket reference for future remediation
- Exception process: Review exceptions quarterly to determine if they can be resolved with Pydantic updates or optimization