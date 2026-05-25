# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Tests Cover Edge

These rules are ALWAYS ACTIVE for all type serializer implementations in pydantic-core/src/serializers/type_serializers/ and their corresponding test suites.

### Rules

- **R-SERIAL-001** MUST: Tests MUST cover edge cases including empty collections, boundary values, type conversion scenarios, and error conditions.
- **R-SERIAL-002** MUST: Type serializer tests MUST NOT introduce mock framework dependencies (e.g., mockall, mock) without documented exception and tech lead approval.
- **R-SERIAL-003** MUST: New type serializer implementations MUST structure tests to instantiate real type instances and verify serialization output directly.
- **R-SERIAL-004** SHOULD: Test cases SHOULD include empty collections, boundary values (min/max), type conversions, and error conditions in every serializer test suite.
- **R-SERIAL-005** SHOULD: Developers SHOULD use Rust's built-in testing framework features (assert_eq!, assert!, #[should_panic]) rather than external testing libraries for serializer tests.
- **R-SERIAL-006** MAY: Property-based testing with generated type instances using frameworks like proptest MAY be adopted as an additional testing layer alongside the mock-free approach to increase coverage of edge cases.

### Verify

```bash
# Check for mock framework imports in type serializer test files
grep -r "mock" pydantic-core/src/serializers/type_serializers/*.rs | wc -l

# Run all serializer tests
cargo test --package pydantic-core --lib serializers::type_serializers

# Count mock-related imports using ripgrep
rg "use.*mock" pydantic-core/src/serializers/type_serializers/ --count
```

**Accept when:**
- Mock framework imports (e.g., 'use mockall', 'use mock') are absent from type serializer test files
- All serializer tests in pydantic-core/src/serializers/type_serializers/ pass successfully with cargo test
- Code review confirms new serializer tests follow the established pattern of direct instantiation with real type instances
- Edge case coverage is documented in test file comments or verified through code coverage analysis

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands MUST execute successfully before accepting changes to type serializer test files. Code review MUST confirm adherence to the mock-free pattern. Any introduction of mock frameworks MUST be blocked unless accompanied by documented exception approval from tech lead.
</enforcement>