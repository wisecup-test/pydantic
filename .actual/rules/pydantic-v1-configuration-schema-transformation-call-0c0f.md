# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: Configuration Schema Transformation Callables Adhere Schemaextracallable

These rules are ALWAYS ACTIVE for internal core library modules responsible for configuration, error definitions, and type introspection.

### Rules

- **R-TYP-001** MUST: Configuration and schema transformation callables MUST adhere to the SchemaExtraCallable protocol or ConfigDict structure defined in the centralized typing and configuration modules.

### Verify

```bash
# Discover and execute the complete test suite across supported runtime environments
pytest
# Run static analysis and type checking on core modules
mypy pydantic/v1
```

**Accept when:**
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>