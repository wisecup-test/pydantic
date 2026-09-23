# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: Type Introspection Routines Evaluating Structured Mappings

These rules are ALWAYS ACTIVE for internal core library modules responsible for configuration, error definitions, and type introspection evaluating structured mappings.

### Rules

- **R-TYP-001** MUST: Type introspection routines evaluating structured mappings MUST use is_typeddict and is_legacy_typeddict from the centralized typing module to resolve attribute requirements across supported runtime environments.

### Verify

```bash
# Discover the project test runner configuration and execute the complete test suite across all supported runtime environments.
# Discover the static analysis and type checking configuration in the repository manifest and execute type verification on the core modules.
```

**Accept when:**
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static type checkers, continuous integration test matrices, and peer code review.
</enforcement>