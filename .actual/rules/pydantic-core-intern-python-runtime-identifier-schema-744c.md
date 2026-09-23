# PyO3 String Interning with pyo3::intern for Python Runtime Identifier and Schema Serialization Lookups: Before Implementing Dependencies Relying Versioned Foreign

These rules are ALWAYS ACTIVE for Rust serialization builders, type serializers interacting with Python runtime attributes or dictionary keys, and common runtime utility modules managing static sentinel tokens or prebuilt schema constants.

### Rules

- **R-PYO3-001** MUST: Before implementing dependencies relying on versioned foreign function interface libraries, developers MUST inspect the repository lock artifact to verify the exact resolved library version and consult that version's authoritative documentation.
- **R-PYO3-002** MUST: Ensure all call sites invoking pyo3::intern pass static string slices corresponding to stable Python attribute and dictionary keys.
- **R-PYO3-003** MUST: Coordinate interning operations with Python interpreter thread state tokens to guarantee valid foreign function interface access.

### Verify

```bash
# Discover workspace dependency manifest and execute test runner
# (Command derived from project repository manifest and build tool)

# Run code linter and static analysis suite
# (Command derived from project repository linter configuration)

# Execute benchmark suite
# (Command derived from project repository build tool)
```

**Accept when:**
- All unit and integration tests across type serializers pass without compilation or runtime errors.
- Static analysis confirms constant string keys across serializer definitions utilize pyo3::intern.
- No redundant Python string allocations are introduced for constant schema identifiers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified through automated continuous integration build checks, test execution, and peer code review.
</enforcement>