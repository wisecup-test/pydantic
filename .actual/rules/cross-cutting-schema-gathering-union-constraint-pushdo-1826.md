# MISSING sentinel unification across schema generation and serialization: Schema Gathering Union Constraint Pushdown Routines

These rules are ALWAYS ACTIVE for schema generation, union constraint pushdown routines, internal schema gathering logic, type serializers, and JSON schema emission layers handling the MISSING sentinel.

### Rules

- **R-MISSING-001** MUST: Schema gathering and union constraint pushdown routines MUST explicitly account for the MISSING sentinel when evaluating discriminated types and union branches.

### Verify

```bash
# Discover and execute test suites targeting union branch resolution, missing sentinel behavior, and model serialization
pytest tests/test_missing_sentinel.py tests/types/test_union.py
# Discover and execute core serializer verification tests across Python and native extensions
cargo test
```

**Accept when:**
- All schema generation tests pass without substituting None for missing attributes
- Model serialization with exclude_unset correctly handles MISSING without exposing the sentinel object
- JSON schema export contains standard JSON types without sentinel leakage

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests leaking MISSING to downstream consumers or using None for absent fields must be rejected at review, and test failures for sentinel leakage or union branch mismatch block CI merges.
</enforcement>