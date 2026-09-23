# Adoption of jiter for JSON Input and Error Value Representation: Validator Implementations Operating Structured Object Inputs

These rules are ALWAYS ACTIVE for validator implementations operating on structured object inputs and JSON input parsing/error reporting pipelines.

### Rules

- **R-JITER-001** SHOULD: Validator implementations operating on structured object inputs SHOULD traverse fields directly via jiter::JsonObject accessor interfaces rather than allocating intermediate associative maps.

### Verify

```bash
# Discover and execute the test suite covering JSON input validation pipelines
# Discover and run static type checking and linting tasks defined in the project build configuration
```

**Accept when:**
- All test suites exercising jiter::JsonValue and jiter::JsonObject input parsing, field validation, and error reporting pass without regression.
- Static verification and linting processes confirm clean compilation with no unresolved type contract violations across validation modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test passes and mandatory peer code review enforce adherence to jiter data structures for JSON evaluation paths, blocking pull requests introducing non-jiter JSON representations or redundant intermediate runtime object conversions.
</enforcement>