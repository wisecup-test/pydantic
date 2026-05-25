# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Serializer Test Suites

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ACTIVE for all type serializer implementations in pydantic-core and applies to testing strategies for serialization components.

## Context

- Type serializers in pydantic-core require consistent testing approaches to ensure correctness across diverse data types including sets, frozensets, timedelta, and sentinel values
- The codebase demonstrates a pattern of testing serializers without relying on mocking frameworks, instead using direct instantiation and real type behavior verification
- Serialization logic is critical for data validation and transformation, requiring high confidence in correctness through comprehensive testing
- The pattern appears consistently across multiple type serializer implementations (set_frozenset.rs, missing_sentinel.rs, timedelta.rs), indicating an established architectural practice
- Testing strategy must accommodate Rust's type system and ownership model while ensuring serializers handle edge cases, type conversions, and error conditions correctly

## Problem Statement

Type serializers form the core of pydantic-core's data transformation capabilities, but inconsistent testing approaches can lead to gaps in coverage, unreliable behavior under edge cases, and difficulty maintaining test suites. The challenge is establishing a standardized testing strategy that validates serializer correctness without introducing brittle mock dependencies while ensuring comprehensive coverage of type-specific behaviors.

## Decision

1. MUST: Serializer test suites MUST verify behavior with real type instances (sets, frozensets, timedelta, sentinel values) rather than simulated data

## Policy Block

- MUST Serializer test suites MUST verify behavior with real type instances (sets, frozensets, timedelta, sentinel values) rather than simulated data

In scope:
- All type serializer implementations in pydantic-core/src/serializers/type_serializers/
- Unit tests for serialization logic including sets, frozensets, timedelta, sentinel values, and future type serializers
- Integration tests that verify serializer behavior within the broader validation pipeline
- Test coverage for edge cases, error conditions, and type conversion scenarios

Out of scope:
- End-to-end system tests that may require different testing strategies
- Performance benchmarks and load testing which may use synthetic data generation
- Tests for non-serializer components where mocking may be appropriate
- External API integration tests where test doubles are necessary

Exceptions:
- EX-001: Testing serializers that interact with external I/O or network resources where real instances are impractical
- EX-002: Performance testing scenarios where generating real type instances would be prohibitively expensive

## Rationale

- Pattern detected across 3 files with 89.47% confidence indicates this is an established and intentional architectural practice in the codebase
- Mock-free testing provides stronger guarantees of correctness by exercising actual type behavior and Rust ownership semantics rather than simulated behavior
- Direct instantiation testing reduces test maintenance burden by eliminating mock setup/teardown and avoiding brittle mock expectations that break with implementation changes
- The approach aligns with Rust's philosophy of compile-time guarantees and type safety, leveraging the type system for test reliability rather than runtime mocking

## Consequences

Positive:
- Higher confidence in serializer correctness through testing with real type instances and actual runtime behavior
- Reduced test maintenance burden by eliminating mock framework dependencies and brittle mock expectations
- Improved test readability and clarity by using straightforward instantiation rather than complex mock setup
- Better alignment with Rust's type system and ownership model, catching potential issues at compile time
- Consistent testing patterns across all type serializers improve developer onboarding and code comprehension

Negative:
- Tests may be slower than mock-based alternatives if real type instantiation is expensive (though unlikely for primitive types)
- Some edge cases involving external dependencies may be harder to test without mocking capabilities
- Developers familiar with mock-heavy testing frameworks may need to adjust their testing mindset
- Limited ability to test certain failure scenarios that are difficult to trigger with real instances

## Alternatives

- Adopt a comprehensive mocking framework (e.g., mockall) for all serializer tests (rejected)
  Rejected because: Introduces unnecessary complexity and dependencies for testing simple type serializers; mocks can create false confidence by simulating behavior that differs from actual runtime behavior; contradicts the established pattern with 89.47% confidence across the codebase
  When valid: Only appropriate for testing components with complex external dependencies like network I/O or database interactions
- Hybrid approach using mocks for complex scenarios and real instances for simple cases (rejected)
  Rejected because: Creates inconsistency in testing strategy making it unclear when to use each approach; increases cognitive load for developers; the current pattern demonstrates that real instance testing is sufficient even for complex types
  When valid: Could be reconsidered if future serializers require interaction with truly unmockable external systems
- Property-based testing with generated type instances using frameworks like proptest (deferred)
  Rejected because: Not rejected but deferred for future consideration; could complement the current approach by generating diverse test cases
  When valid: Could be adopted as an additional testing layer alongside the current mock-free approach to increase coverage of edge cases

## Risks

- Future serializers with complex external dependencies may be difficult to test without mocking capabilities
  Mitigation: Document exception process for cases where real instance testing is impractical; evaluate property-based testing or test fixture approaches before resorting to mocks
  Owner: Engineering team / Test strategy lead
- Developers from mock-heavy backgrounds may introduce inconsistent testing patterns
  Mitigation: Include testing strategy in onboarding documentation; enforce pattern through code review; provide examples from existing serializer tests as templates
  Owner: Engineering team / Code reviewers
- Edge cases that are difficult to trigger with real instances may have insufficient test coverage
  Mitigation: Supplement with property-based testing where appropriate; document known coverage gaps; use code coverage tools to identify untested paths
  Owner: QA team / Engineering team

## Implementation Notes

- Review existing serializer tests in set_frozenset.rs, missing_sentinel.rs, and timedelta.rs as reference implementations for the established pattern
- When creating new type serializers, structure tests to instantiate real type instances and verify serialization output directly
- Include test cases for empty collections, boundary values (min/max), type conversions, and error conditions in every serializer test suite
- Use Rust's built-in testing framework features (assert_eq!, assert!, #[should_panic]) rather than external testing libraries
- Document any deviations from the mock-free pattern with clear justification in test file comments

## Continuation Context


Verify commands:
- grep -r "mock" pydantic-core/src/serializers/type_serializers/*.rs | wc -l
- cargo test --package pydantic-core --lib serializers::type_serializers
- rg "use.*mock" pydantic-core/src/serializers/type_serializers/ --count

Accept when:
- Mock framework imports (e.g., 'use mockall', 'use mock') are absent from type serializer test files
- All serializer tests in pydantic-core/src/serializers/type_serializers/ pass successfully with cargo test
- Code review confirms new serializer tests follow the established pattern of direct instantiation with real type instances

## Enforcement

- Verified by: Automated CI pipeline runs cargo test to verify all serializer tests pass
- Verified by: Code review checklist includes verification that new serializer tests follow mock-free pattern
- Verified by: Static analysis checks for mock framework imports in type serializer test files
- Violation handling: CI build fails if mock framework dependencies are introduced in serializer test files without documented exception
- Violation handling: Code review blocks merge if tests use mocking without tech lead approval and justification
- Violation handling: Quarterly audit of test patterns identifies and flags deviations for remediation
- Exception process: Developer documents specific reason why mock-free testing is impractical for the use case
- Exception process: Tech lead or test strategy lead reviews and approves exception with written justification
- Exception process: Exception is documented in test file comments and tracked in ADR exceptions log
- Exception process: Exception is reviewed during quarterly architecture review for potential pattern updates