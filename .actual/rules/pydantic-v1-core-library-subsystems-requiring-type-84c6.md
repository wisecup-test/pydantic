# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: Core Library Subsystems Requiring Type Introspection

These rules are ALWAYS ACTIVE for all core library modules responsible for configuration, error definitions, and type introspection.

### Rules

- **R-TYP-001** MUST: All core library subsystems requiring type introspection, protocol definitions, or runtime type shims MUST import shared typing abstractions from pydantic.v1.typing rather than defining local compatibility constructs.

### Verify

```bash
# Discover the project test runner configuration and execute the complete test suite across all supported runtime environments.
# Discover the static analysis and type checking configuration in the repository manifest and execute type verification on the core modules.
```

**Accept when:**
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static type checkers and linter rules govern internal module imports, and violations result in rejected pull requests.
</enforcement>