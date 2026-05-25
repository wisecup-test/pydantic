# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Tests Use Helper

These rules are ALWAYS ACTIVE for all type serializer implementations in pydantic-core and their corresponding test suites in `pydantic-core/src/serializers/type_serializers/`.

### Rules

- **R-SER-001** MAY: Tests MAY use helper functions or test utilities to reduce duplication across similar serializer test suites.
- **R-SER-002** MUST: Type serializer tests MUST NOT introduce mock framework dependencies (e.g., `mockall`, `mock`) without documented exception and tech lead approval.
- **R-SER-003** SHOULD: Serializer tests SHOULD instantiate real type instances and verify serialization output directly rather than using mocks.
- **R-SER-004** SHOULD: Test cases SHOULD include empty collections, boundary values (min/max), type conversions, and error conditions for every serializer test suite.
- **R-SER-005** SHOULD: Tests SHOULD use Rust's built-in testing framework features (`assert_eq!`, `assert!`, `#[should_panic]`) rather than external testing libraries.

### Verify

```bash
# Check for mock framework imports in type serializer test files
grep -r "mock" pydantic-core/src/serializers/type_serializers/*.rs | wc -l

# Run all serializer tests to verify they pass
cargo test --package pydantic-core --lib serializers::type_serializers

# Count mock-related imports using ripgrep
rg "use.*mock" pydantic-core/src/serializers/type_serializers/ --count
```

**Accept when:**
- Mock framework imports (e.g., `use mockall`, `use mock`) are absent from type serializer test files
- All serializer tests in `pydantic-core/src/serializers/type_serializers/` pass successfully with `cargo test`
- Code review confirms new serializer tests follow the established pattern of direct instantiation with real type instances
- Any deviations from the mock-free pattern are documented with clear justification in test file comments

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands MUST execute successfully before accepting changes to type serializer test files. Code review MUST confirm adherence to the mock-free pattern unless an approved exception is documented.
</enforcement>