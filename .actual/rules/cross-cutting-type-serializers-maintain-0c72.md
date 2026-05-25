# Adopt Rust Type Serializers for Domain Model Serialization: Type Serializers Maintain

These rules are ALWAYS ACTIVE for all domain models and type serializers in pydantic-core requiring serialization logic, particularly those in `pydantic-core/src/serializers/type_serializers/` and related domain model definitions.

### Rules

- **R-TS-001** SHOULD: Type serializers SHOULD maintain separation of concerns between domain model definitions and serialization logic.

### Verify

```bash
# Count type_serializers references in the serializers directory
grep -r "type_serializers" pydantic-core/src/serializers/ | wc -l

# List all type serializer implementation files
find pydantic-core/src/serializers/type_serializers -name "*.rs" -type f

# Run type serializer test suite
cargo test --package pydantic-core --lib serializers::type_serializers
```

**Accept when:**
- Type serializer modules exist in `pydantic-core/src/serializers/type_serializers/` for all domain types requiring specialized serialization
- All type serializers have corresponding test coverage with passing tests
- Grep command returns at least 3 type serializer files (model.rs, timedelta.rs, missing_sentinel.rs)
- Cargo test suite passes with no failures

<enforcement>
Claude Code MUST NOT skip or defer verification. All type serializer implementations MUST pass the verify commands before acceptance. Code review MUST block merges if domain types lack appropriate type serializers or if serialization logic is embedded directly in domain model definitions.
</enforcement>