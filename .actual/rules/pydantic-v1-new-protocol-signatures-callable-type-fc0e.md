# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: New Protocol Signatures Callable Type Specifications

These rules are ALWAYS ACTIVE for all internal core library modules responsible for configuration, error definitions, and type introspection.

### Rules

- **R-PNT-001** SHOULD: New protocol signatures and callable type specifications exposed across module boundaries SHOULD be registered in the shared typing module to prevent type drift.

### Verify

```bash
# Discover and run the complete test suite across all supported runtime environments
pytest
# Discover and run static analysis/type checking
mypy pydantic/
```

**Accept when:**
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>