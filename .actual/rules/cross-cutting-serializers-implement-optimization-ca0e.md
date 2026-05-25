# Adopt Type-Specific Serializer Pattern for Public API Contracts: Serializers Implement Optimization

These rules are ALWAYS ACTIVE for all public API serialization implementations. All type serializers exposed through public APIs MUST conform to these patterns.

### Rules

- **R-SER-001** MAY: Serializers MAY implement optimization strategies specific to their type category.

### Verify

```bash
# Verify type serializer module structure
find . -path '*/serializers/type_serializers/*.rs' -type f | wc -l | grep -E '[0-9]+'

# Verify central coordinator module exists
grep -r 'mod.*serializer' */serializers/type_serializers/mod.rs

# Run serializer tests
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- All type serializers are implemented in separate modules under type_serializers directory
- Central coordinator module (mod.rs) exists and registers all type-specific serializers
- All serializer tests pass with coverage above 80% for serialization logic

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if new types are serialized without dedicated type serializer. Code review MUST block merge if serialization logic is added outside type_serializers module. Architecture review is required for any exceptions to the pattern.
</enforcement>