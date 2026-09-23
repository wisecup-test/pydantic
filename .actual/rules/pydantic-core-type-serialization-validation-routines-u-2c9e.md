# PyO3 Adoption for Python Dictionary and Object Interoperability in Type Serializers and Validators: Type Serialization Validation Routines Use Pyo3

These rules are ALWAYS ACTIVE for all native type serializers, validator components interfacing with Python runtime objects, and modules constructing or traversing Python dictionary structures across language boundaries.

### Rules

- **R-PYO3-001** MUST: Type serialization and validation routines MUST use PyO3 type interfaces, specifically pyo3::types::PyDict for dictionary interoperability, as the primary Python runtime binding abstraction.

### Verify

```bash
# Discover and execute the repository unit and integration test suite targeting serialization and validation modules.
# Discover and execute the repository static analysis and linting verification suite to validate PyO3 type binding compliance.
# (Run the project's standard test and lint commands for serialization/validation modules)
```

**Accept when:**
- All unit and integration tests across type serializers and validators pass without errors.
- Code analysis passes with zero warnings related to PyO3 type conversions, reference counting, or FFI safety.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing raw FFI calls or bypassing PyO3 abstractions will be blocked until refactored. Compiler or linter warnings regarding PyO3 safety invariants will trigger build failure.
</enforcement>