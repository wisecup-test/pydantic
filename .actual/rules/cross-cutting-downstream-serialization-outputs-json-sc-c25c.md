# MISSING sentinel unification across schema generation and serialization: Downstream Serialization Outputs Json Schema Generators

These rules are ALWAYS ACTIVE for all schema generation, serialization, and JSON schema emission files.

### Rules

- **R-MISSING-001** MUST_NOT: Downstream serialization outputs and JSON schema generators MUST NOT leak the internal MISSING sentinel object into user-facing output payloads or exported schemas.

### Verify

```bash
# Discover and execute test suites targeting union branch resolution, missing sentinel behavior, and model serialization
pytest tests/test_missing_sentinel.py tests/types/test_union.py
```

**Accept when:**
- All schema generation tests pass without substituting None for missing attributes
- Model serialization with exclude_unset correctly handles MISSING without exposing the sentinel object
- JSON schema export contains standard JSON types without sentinel leakage

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests leaking MISSING to downstream consumers or using None for absent fields must be rejected at review.
</enforcement>