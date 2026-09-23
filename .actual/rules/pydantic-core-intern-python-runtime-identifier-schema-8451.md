# PyO3 String Interning with pyo3::intern for Python Runtime Identifier and Schema Serialization Lookups: Serializers Runtime Helpers Declare Static String

These rules are ALWAYS ACTIVE for Rust serialization builders, type serializers, and runtime utility modules interacting with Python runtime attributes, dictionary keys, or sentinel tokens.

### Rules

- **R-PYO3-INTERN-001** MUST: Serializers and runtime helpers MUST declare static string literals through pyo3::intern at the point of attribute retrieval or dictionary field extraction.

### Verify

```bash
# Discover workspace dependency manifest and execute test runner for serializer modules
# Discover and run code linter and static analysis suite
# Execute benchmark suite via project build tool
```

**Accept when:**
- All unit and integration tests across type serializers pass without compilation or runtime errors.
- Static analysis confirms constant string keys across serializer definitions utilize pyo3::intern.
- No redundant Python string allocations are introduced for constant schema identifiers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration build checks and peer code reviews enforce compliance.
</enforcement>