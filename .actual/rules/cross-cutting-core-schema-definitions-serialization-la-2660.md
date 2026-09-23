# MISSING sentinel unification across schema generation and serialization: Core Schema Definitions Serialization Layers Represent

These rules are ALWAYS ACTIVE for all core schema generation, serialization, type serializer implementations, and JSON schema emission logic.

### Rules

- **R-MISSING-001** MUST: Core schema definitions and serialization layers MUST represent absent or omitted attributes using the explicit MISSING sentinel rather than None.

### Verify

```bash
# Discover and execute test suites targeting union branch resolution, missing sentinel behavior, and model serialization
pytest tests/test_missing_sentinel.py tests/types/test_union.py
# Discover and execute core serializer verification tests across Python and native extensions
pytest tests/ -k "serializer"
```

**Accept when:**
- All schema generation tests pass without substituting None for missing attributes
- Model serialization with exclude_unset correctly handles MISSING without exposing the sentinel object
- JSON schema export contains standard JSON types without sentinel leakage

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests leaking MISSING to downstream consumers or using None for absent fields must be rejected at review.
</enforcement>