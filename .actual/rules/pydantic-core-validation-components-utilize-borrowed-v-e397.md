# Adoption of jiter for JSON Input and Error Value Representation: Validation Components Utilize Borrowed Variants Jiter

These rules are ALWAYS ACTIVE for validation components processing JSON input and error value representations.

### Rules

- **R-JITER-001** MAY: Validation components MAY utilize borrowed variants of jiter data structures to avoid heap allocations when input lifetimes allow.

### Verify

```bash
# Discover the project verification script from the manifest and execute the test suite covering JSON input validation pipelines.
# Discover and run the static type checking and linting tasks defined in the project build configuration to verify jiter type contract compliance.
```

**Accept when:**
- All test suites exercising jiter::JsonValue and jiter::JsonObject input parsing, field validation, and error reporting pass without regression.
- Static verification and linting processes confirm clean compilation with no unresolved type contract violations across validation modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test passes and mandatory peer code review verify adherence to jiter data structures.
</enforcement>