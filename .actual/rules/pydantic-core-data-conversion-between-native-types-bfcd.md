# PyO3 Adoption for Python Dictionary and Object Interoperability in Type Serializers and Validators: Data Conversion Between Native Types Python

These rules are ALWAYS ACTIVE for native type serializers, validators, and modules constructing or traversing Python dictionary structures across language boundaries.

### Rules

- **R-PYO3-001** MUST: Data conversion between native types and Python runtime structures MUST implement pyo3::IntoPyObjectExt or pyo3::pybacked::PyBackedStr rather than manual FFI pointer manipulation.

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