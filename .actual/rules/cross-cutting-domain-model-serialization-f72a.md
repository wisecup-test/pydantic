# Adopt Rust Type Serializers for Domain Model Serialization: Domain Model Serialization

These rules are ALWAYS ACTIVE for all domain models requiring serialization in pydantic-core, custom domain types with specialized serialization requirements, temporal domain types, sentinel and special marker values, and complex nested domain model structures.

### Rules

- **R-RUST-SERIALIZERS-001** MUST: Domain model serialization MUST be implemented using dedicated Rust type serializers in the pydantic-core serializers/type_serializers directory.

### Verify

```bash
# Count type_serializers references in the serializers directory
grep -r "type_serializers" pydantic-core/src/serializers/ | wc -l

# List all type serializer files
find pydantic-core/src/serializers/type_serializers -name "*.rs" -type f

# Run type serializer tests
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- Type serializer modules exist in pydantic-core/src/serializers/type_serializers/ for all domain types requiring specialized serialization
- All type serializers have corresponding test coverage with passing tests
- Grep command returns at least 3 type serializer files (model.rs, timedelta.rs, missing_sentinel.rs)
- Cargo test command completes successfully with all tests passing

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializer implementations MUST pass the verify commands before acceptance. Code review MUST check for proper type serializer implementation. Architecture review MUST flag domain models with inline serialization logic as violations.
</enforcement>