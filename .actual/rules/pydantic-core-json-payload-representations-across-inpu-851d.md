# Adoption of jiter for JSON Input and Error Value Representation: Json Payload Representations Across Input Abstraction

These rules are ALWAYS ACTIVE for all JSON input parsing, field validation, and error reporting components processing structured JSON payloads within the validation engine.

### Rules

- **R-JITER-001** MUST: All JSON payload representations across input abstraction, field validation, and line error reporting layers MUST use jiter data types, specifically jiter::JsonValue and jiter::JsonObject, as the canonical internal JSON data representation.

### Verify

```bash
# Discover the project verification script from the manifest and execute the test suite covering JSON input validation pipelines.
# Discover and run the static type checking and linting tasks defined in the project build configuration to verify jiter type contract compliance.
```

**Accept when:**
- All test suites exercising jiter::JsonValue and jiter::JsonObject input parsing, field validation, and error reporting pass without regression.
- Static verification and linting processes confirm clean compilation with no unresolved type contract violations across validation modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Adherence to jiter data structures for JSON evaluation paths is mandatory and enforced via continuous integration and peer code review.
</enforcement>