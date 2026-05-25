# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Test Implementations Follow

These rules are ALWAYS ACTIVE for all type serializer implementations in pydantic-core/src/serializers/type_serializers/ and their corresponding unit and integration tests.

### Rules

- **R-SERIAL-001** SHOULD: Test implementations SHOULD follow the established pattern observed in set_frozenset.rs, missing_sentinel.rs, and timedelta.rs serializer tests, using direct instantiation of real type instances rather than mocking frameworks.

- **R-SERIAL-002** MUST: Type serializer tests MUST NOT introduce mock framework imports (e.g., 'use mockall', 'use mock') without documented exception and tech lead approval.

- **R-SERIAL-003** SHOULD: Serializer test suites SHOULD include test cases for empty collections, boundary values (min/max), type conversions, and error conditions.

- **R-SERIAL-004** SHOULD: Tests SHOULD use Rust's built-in testing framework features (assert_eq!, assert!, #[should_panic]) rather than external testing libraries.

- **R-SERIAL-005** SHOULD: Any deviations from the mock-free pattern SHOULD be documented with clear justification in test file comments.

### Verify

```bash
# Check for mock framework imports in type serializer test files
grep -r "mock" pydantic-core/src/serializers/type_serializers/*.rs | wc -l

# Verify all serializer tests pass
cargo test --package pydantic-core --lib serializers::type_serializers

# Count mock-related imports using ripgrep
rg "use.*mock" pydantic-core/src/serializers/type_serializers/ --count
```

**Accept when:**
- Mock framework imports (e.g., 'use mockall', 'use mock') are absent from type serializer test files
- All serializer tests in pydantic-core/src/serializers/type_serializers/ pass successfully with cargo test
- Code review confirms new serializer tests follow the established pattern of direct instantiation with real type instances
- Any exceptions are documented in test file comments with written justification from tech lead

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST run cargo test to verify all serializer tests pass. Code review MUST include verification that new serializer tests follow mock-free pattern. Static analysis MUST check for mock framework imports in type serializer test files. Violations block merge without documented exception and tech lead approval.
</enforcement>