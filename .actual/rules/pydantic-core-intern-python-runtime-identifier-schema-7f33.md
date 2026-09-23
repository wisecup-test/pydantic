# PyO3 String Interning with pyo3::intern for Python Runtime Identifier and Schema Serialization Lookups: Components Interacting Constant Python String Keys

These rules are ALWAYS ACTIVE for components interacting with constant Python string keys, attribute names, or runtime sentinels across serialization builders and core utilities.

### Rules

- **R-INT-001** MUST: Components interacting with constant Python string keys, attribute names, or runtime sentinels MUST obtain Python string references using pyo3::intern rather than dynamic string allocation routines.
- **R-EX-001** EXCEPTION: A target Python string is dynamically computed at runtime and cannot be represented as a static string literal.

### Verify

```bash
# Discover the workspace dependency manifest and execute the test runner to ensure serializer modules compile and pass all tests.
# Discover and run the project code linter and static analysis suite to verify compliance with string interning rules.
# Execute the benchmark suite through the project build tool to validate that serialization overhead remains within expected performance boundaries.
```

**Accept when:**
- All unit and integration tests across type serializers pass without compilation or runtime errors.
- Static analysis confirms constant string keys across serializer definitions utilize pyo3::intern.
- No redundant Python string allocations are introduced for constant schema identifiers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration build checks and peer code reviews.
</enforcement>