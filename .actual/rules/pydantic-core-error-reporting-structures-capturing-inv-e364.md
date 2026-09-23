# Adoption of jiter for JSON Input and Error Value Representation: Error Reporting Structures Capturing Invalid Json

These rules are ALWAYS ACTIVE for JSON input parsing, field validation routines, and error reporting pipelines processing structured JSON objects.

### Rules

- **R-JITER-001** MUST: Error reporting structures capturing invalid JSON values MUST store line-level error contexts using jiter::JsonValue representations to preserve input fidelity.

### Verify

```bash
# Discover and execute the project verification script from the manifest covering JSON input validation pipelines
# Discover and run static type checking and linting tasks defined in the project build configuration
```

**Accept when:**
- All test suites exercising jiter::JsonValue and jiter::JsonObject input parsing, field validation, and error reporting pass without regression.
- Static verification and linting processes confirm clean compilation with no unresolved type contract violations across validation modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test passes and mandatory peer code review verify adherence to jiter data structures for JSON evaluation paths.
</enforcement>