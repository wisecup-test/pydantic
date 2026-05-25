# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Type Serializer Tests

These rules are ALWAYS ACTIVE for all type serializer implementations in pydantic-core/src/serializers/type_serializers/ and their corresponding unit and integration tests.

### Rules

- **R-SERIALIZER-001** MUST_NOT: Type serializer tests MUST NOT introduce dependencies on external mocking frameworks that could create test brittleness or maintenance burden.

### Verify

```bash
# Check for mock framework imports in type serializer test files
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
- Tests instantiate real type instances and verify serialization output directly
- Test cases cover empty collections, boundary values (min/max), type conversions, and error conditions
- Tests use Rust's built-in testing framework features (assert_eq!, assert!, #[should_panic]) rather than external testing libraries

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializer tests MUST be reviewed to ensure compliance with the mock-free pattern before approval. Violations without documented exceptions and tech lead approval MUST block merge.
</enforcement>