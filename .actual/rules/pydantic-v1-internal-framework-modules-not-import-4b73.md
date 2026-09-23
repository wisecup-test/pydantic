# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: Internal Framework Modules Not Import Diverging

These rules are ALWAYS ACTIVE for internal core library modules responsible for configuration, error definitions, and type introspection.

### Rules

- **R-TYP-001** MUST_NOT: Internal framework modules MUST NOT import diverging platform-specific typing constructs directly when an equivalent shim is maintained within the centralized typing layer.

### Verify

```bash
# Discover the project test runner configuration and execute the complete test suite across all supported runtime environments.
# Discover the static analysis and type checking configuration in the repository manifest and execute type verification on the core modules.
pytest
mypy .
```

**Accept when:**
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static type checkers and linter rules enforce internal module imports. Violation handling: Pull requests importing divergent platform typing constructs directly when a shared shim exists are rejected.
</enforcement>