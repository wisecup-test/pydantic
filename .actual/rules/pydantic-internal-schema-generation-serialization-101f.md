# Adoption of pydantic_core for Schema Generation, Validation, and Serialization Core Contracts: Internal Schema Generation Serialization Subsystems Delegate

These rules are ALWAYS ACTIVE for internal schema generation, signature parsing, and serialization handler modules that construct or consume CoreSchema definitions, and surrogate validator and serializer components that proxy validation execution or handle deferred rebuilding.

### Rules

- **R-PYD-001** MUST: Internal schema generation and serialization subsystems MUST delegate core validation and serialization execution to pydantic_core interfaces and data representations.

### Verify

```bash
# Discover the project test runner and execute test suites targeting core schema generation, reference resolution, and serialization pipelines.
pytest tests/test_core_schema.py tests/test_validation.py tests/test_serialization.py

# Discover the repository static analysis and type checking tools and run validation checks across internal handler modules interfacing with core schema protocols.
mypy src/internal_handlers/ src/schema_generation/
```

**Accept when:**
- All schema generation and reference resolution test suites pass without unresolved reference errors.
- Static type checking confirms all mock proxies and handler callbacks conform to expected core schema interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory for all modifications touching core schema handlers or validation proxies.
</enforcement>