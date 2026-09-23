# PyO3 String Interning with pyo3::intern for Python Runtime Identifier and Schema Serialization Lookups: Serialization Routines Not Construct Dynamic Python

These rules are ALWAYS ACTIVE for Rust serialization builders, type serializers interacting with Python runtime attributes/dictionary keys, and common runtime utility modules managing static sentinel tokens or prebuilt schema constants.

### Rules

- **R-INT-001** MUST_NOT: Serialization routines MUST NOT construct dynamic Python string instances for constant field names or known schema keys.

### Verify

```bash
# Discover workspace dependency manifest and execute test runner
# Discover and run the project code linter and static analysis suite
# Execute the benchmark suite through the project build tool
```

**Accept when:**
- All unit and integration tests across type serializers pass without compilation or runtime errors.
- Static analysis confirms constant string keys across serializer definitions utilize pyo3::intern.
- No redundant Python string allocations are introduced for constant schema identifiers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>