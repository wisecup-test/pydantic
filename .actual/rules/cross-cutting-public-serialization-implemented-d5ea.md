# Adopt Type-Specific Serializer Pattern for Public API Contracts: Public Serialization Implemented

These rules are ALWAYS ACTIVE for all public API serialization implementations. All type serializers exposed through public APIs MUST conform to these patterns.

### Rules

- **R-PSI-001** MUST: All public API serialization MUST be implemented through dedicated type-specific serializer modules.
- **R-PSI-002** MUST: Create a central serializer registry module (mod.rs) that coordinates type-to-serializer mapping.
- **R-PSI-003** MUST: Implement each type serializer in its own file following naming convention: {type_name}.rs.
- **R-PSI-004** MUST: Ensure all serializers implement a common trait or interface for consistent invocation.
- **R-PSI-005** MUST: Add comprehensive unit tests for each type serializer covering edge cases and error conditions.
- **R-PSI-006** MUST: Document the serialization format for each type in API documentation to establish clear contracts.
- **R-PSI-007** SHOULD: Establish clear guidelines for when new type serializers are warranted vs. using existing generic handlers.
- **R-PSI-008** MAY: Consider trait-based serialization as a complementary approach for types with natural serialization representations.

### Verify

```bash
# Count type serializer modules
find . -path '*/serializers/type_serializers/*.rs' -type f | wc -l | grep -E '[0-9]+'

# Verify central coordinator module exists and registers serializers
grep -r 'mod.*serializer' */serializers/type_serializers/mod.rs

# Run serializer tests
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- All type serializers are implemented in separate modules under type_serializers directory
- Central coordinator module (mod.rs) exists and registers all type-specific serializers
- All serializer tests pass with coverage above 80% for serialization logic
- Each type serializer has comprehensive unit tests covering edge cases and error conditions
- Serialization formats are documented in API documentation

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for public API serialization implementations. CI pipeline MUST fail if new types are serialized without dedicated type serializer. Code review MUST block merge if serialization logic is added outside type_serializers module.
</enforcement>