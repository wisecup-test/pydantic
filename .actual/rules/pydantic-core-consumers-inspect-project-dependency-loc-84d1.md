# Adoption of jiter for JSON Input and Error Value Representation: Consumers Inspect Project Dependency Lock Artifact

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-JITER-001** MUST: Consumers MUST inspect the project dependency lock artifact to discover and resolve the exact locked version of jiter prior to integrating or modifying JSON evaluation paths.

### Verify

```bash
# Discover the project verification script from the manifest and execute the test suite covering JSON input validation pipelines.
# Discover and run the static type checking and linting tasks defined in the project build configuration to verify jiter type contract compliance.
```

**Accept when:**
- All test suites exercising jiter::JsonValue and jiter::JsonObject input parsing, field validation, and error reporting pass without regression.
- Static verification and linting processes confirm clean compilation with no unresolved type contract violations across validation modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>