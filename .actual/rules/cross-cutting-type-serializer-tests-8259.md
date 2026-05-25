# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Type Serializer Tests

These rules are ALWAYS ACTIVE for all type serializer implementations in pydantic-core/src/serializers/type_serializers/ and their corresponding unit and integration tests.

### Rules

- **R-SERIALIZER-001** MUST: Type serializer tests MUST use direct instantiation of serializer types rather than mock objects or test doubles.

### Verify

```bash
# Check for absence of mock framework imports in type serializer test files
grep -r "mock" pydantic-core/src/serializers/type_serializers/*.rs | wc -l

# Run all serializer tests to verify they pass
cargo test --package pydantic-core --lib serializers::type_serializers

# Verify no mock framework usage patterns
rg "use.*mock" pydantic-core/src/serializers/type_serializers/ --count
```

**Accept when:**
- Mock framework imports (e.g., 'use mockall', 'use mock') are absent from type serializer test files
- All serializer tests in pydantic-core/src/serializers/type_serializers/ pass successfully with cargo test
- Code review confirms new serializer tests follow the established pattern of direct instantiation with real type instances
- Test cases cover empty collections, boundary values (min/max), type conversions, and error conditions
- Tests use Rust's built-in testing framework features (assert_eq!, assert!, #[should_panic]) rather than external mocking libraries

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializer tests MUST be reviewed to confirm mock-free patterns are followed. Violations require documented exceptions approved by tech lead with written justification.
</enforcement>