# Standardize Serializer Testing with Mock-Free Type Serializer Patterns: Serializer Test Suites

These rules are ALWAYS ACTIVE for all type serializer implementations in pydantic-core and their corresponding test suites.

### Rules

- **R-SER-001** MUST: Serializer test suites MUST verify behavior with real type instances (sets, frozensets, timedelta, sentinel values) rather than simulated data.
- **R-SER-002** MUST: Type serializer test files MUST NOT import or use mocking frameworks (e.g., mockall, mock) without documented exception approval.
- **R-SER-003** MUST: New type serializer implementations MUST include test cases for empty collections, boundary values (min/max), type conversions, and error conditions.
- **R-SER-004** SHOULD: Serializer tests SHOULD use Rust's built-in testing framework features (assert_eq!, assert!, #[should_panic]) rather than external testing libraries.
- **R-SER-005** SHOULD: Deviations from the mock-free pattern SHOULD be documented with clear justification in test file comments.
- **R-SER-006** MAY: Property-based testing with generated type instances using frameworks like proptest MAY be adopted as a complementary testing layer alongside the mock-free approach.

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
- Mock framework imports (e.g., 'use mockall', 'use mock') are absent from type serializer test files
- All serializer tests in pydantic-core/src/serializers/type_serializers/ pass successfully with cargo test
- Code review confirms new serializer tests follow the established pattern of direct instantiation with real type instances
- Any deviations from the mock-free pattern are documented with written justification and tech lead approval

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST run cargo test to verify all serializer tests pass. Code review MUST include verification that new serializer tests follow the mock-free pattern. Static analysis MUST check for mock framework imports in type serializer test files. Violations block merge without documented exception and tech lead approval.
</enforcement>