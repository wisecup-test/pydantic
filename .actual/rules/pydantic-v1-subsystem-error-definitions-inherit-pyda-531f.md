# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: Subsystem Error Definitions Inherit Pydanticerrormixin Extend

These rules are ALWAYS ACTIVE for internal core library modules responsible for configuration, error definitions, and type introspection.

### Rules

- **R-TYP-001** MUST: Subsystem error definitions MUST inherit from PydanticErrorMixin and extend appropriate base exception classes while formatting error messages through standard instance template mapping.

### Verify

```bash
# Discover and execute the project test runner configuration across all supported runtime environments
pytest
# Discover and execute static analysis and type checking configuration in the repository manifest
mypy src/
```

**Accept when:**
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>