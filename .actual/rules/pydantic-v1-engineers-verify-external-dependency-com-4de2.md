# Centralization of Type Introspection and Compatibility Layer in pydantic.v1.typing: Engineers Verify External Dependency Compatibility Against

These rules are ALWAYS ACTIVE for internal core library modules responsible for configuration, error definitions, type introspection, protocol specifications, and structural dictionary validation routines.

### Rules

- **R-TYP-001** MUST: Engineers MUST verify external dependency compatibility against the project dependency lock artifact before introducing or modifying typing shims to ensure aligned interface expectations.

### Verify

```bash
# Discover the project test runner configuration and execute the complete test suite across all supported runtime environments.
# Discover the static analysis and type checking configuration in the repository manifest and execute type verification on the core modules.
```

**Accept when:**
- All unit and integration test suites pass without runtime type errors across all supported execution environments.
- Static type analysis validates that all cross-module protocols and configuration dictionaries conform to shared typing declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static type checkers, linter rules governing internal module imports, and continuous integration test matrices execute verification. Pull requests importing divergent platform typing constructs directly when a shared shim exists are rejected.
</enforcement>