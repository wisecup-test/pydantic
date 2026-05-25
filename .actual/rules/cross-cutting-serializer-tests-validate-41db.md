# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Serializer Tests Validate

These rules are ALWAYS ACTIVE for all type serializer implementations in pydantic-core/src/serializers/type_serializers/ and their corresponding test suites.

### Rules

- **R-SER-001** SHOULD: Serializer tests SHOULD validate both successful serialization paths and error handling for invalid inputs.

### Verify

```bash
# Check for absence of mock framework imports in type serializer test files
grep -r "mock" pydantic-core/src/serializers/type_serializers/*.rs | wc -l

# Run all serializer tests to verify they pass
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify no mock framework usage in type serializers
rg "use.*mock" pydantic-core/src/serializers/type_serializers/ --count
```

**Accept when:**
- Mock framework imports (e.g., 'use mockall', 'use mock') are absent from type serializer test files
- All serializer tests in pydantic-core/src/serializers/type_serializers/ pass successfully with cargo test
- Code review confirms new serializer tests follow the established pattern of direct instantiation with real type instances
- Tests include coverage for empty collections, boundary values, type conversions, and error conditions
- Tests use Rust's built-in testing framework features (assert_eq!, assert!, #[should_panic]) rather than external mocking libraries

<enforcement>
Claude Code MUST NOT skip or defer verification. All serializer tests MUST be validated against these rules before acceptance. Violations require documented exceptions approved by tech lead or test strategy lead.
</enforcement>