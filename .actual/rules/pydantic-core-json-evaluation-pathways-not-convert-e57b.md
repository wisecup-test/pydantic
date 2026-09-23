# Adoption of jiter for JSON Input and Error Value Representation: Json Evaluation Pathways Not Convert Jiter

These rules are ALWAYS ACTIVE for JSON input parsing, field validation routines, and error reporting pipelines processing structured JSON objects.

### Rules

- **R-JITER-001** MUST_NOT: JSON evaluation pathways MUST NOT convert jiter::JsonValue instances into intermediate dynamic foreign-function objects prior to schema validation.

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