# Adoption of pydantic_core as Core Schema and Validation Engine: Internal Modules Not Bypass Pydantic Core

These rules are ALWAYS ACTIVE for internal modules responsible for model construction, schema gathering, and schema generation, components producing or transforming core schema representations for validation and serialization, and mocking structures managing deferred model initialization and validation rebuilding.

### Rules

- **R-PYDANTIC-001** MUST_NOT: Internal modules MUST NOT bypass pydantic_core schema structures to perform raw validation or custom serialization on core model definitions.

### Verify

```bash
# Discover and execute the project test suite and static type analysis across internal schema generation modules
pytest
mypy src/
```

**Accept when:**
- All schema generation, model construction, and reference resolution tests pass against the resolved validation engine.
- Static analysis confirms all core schema transformations adhere strictly to pydantic_core schema protocol definitions.
- Deferred model construction properly wraps validator and serializer access via mock instances without unhandled errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated test suites and static type checking.
</enforcement>