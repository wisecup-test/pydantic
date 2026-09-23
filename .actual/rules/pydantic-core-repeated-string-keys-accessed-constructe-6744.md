# PyO3 Adoption for Python Dictionary and Object Interoperability in Type Serializers and Validators: Repeated String Keys Accessed Constructed Across

These rules are ALWAYS ACTIVE for all native type serializers and validator components interacting with Python runtime objects.

### Rules

- **R-PYO3-001** SHOULD: Repeated string keys accessed or constructed across dictionary serialization and validation routines MUST use pyo3::intern to avoid redundant Python string allocations.

### Verify

```bash
# Discover and execute the repository unit and integration test suite targeting serialization and validation modules.
# Discover and execute the repository static analysis and linting verification suite to validate PyO3 type binding compliance.
```

**Accept when:**
- All unit and integration tests across type serializers and validators pass without errors.
- Code analysis passes with zero warnings related to PyO3 type conversions, reference counting, or FFI safety.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>