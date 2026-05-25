# Adopt Type-Specific Serializer Pattern for Public API Contracts: Type Serializers Implemented

These rules are ALWAYS ACTIVE for all public API serialization implementations. All type serializers exposed through public APIs MUST conform to these patterns.

### Rules

- **R-SERIALIZER-001** SHOULD: Type serializers SHOULD be implemented in separate files to maintain clear separation of concerns.

### Verify

```bash
# Count type serializer files in the designated directory
find . -path '*/serializers/type_serializers/*.rs' -type f | wc -l | grep -E '[0-9]+'

# Verify central coordinator module exists and registers serializers
grep -r 'mod.*serializer' */serializers/type_serializers/mod.rs

# Run serializer tests with coverage verification
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- All type serializers are implemented in separate modules under type_serializers directory
- Central coordinator module (mod.rs) exists and registers all type-specific serializers
- All serializer tests pass with coverage above 80% for serialization logic

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if new types are serialized without dedicated type serializer. Code review MUST block merge if serialization logic is added outside type_serializers module. Architecture review is required for any exceptions to the pattern.
</enforcement>