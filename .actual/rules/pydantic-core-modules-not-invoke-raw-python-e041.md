# PyO3 Adoption for Python Dictionary and Object Interoperability in Type Serializers and Validators: Modules Not Invoke Raw Python Functions

These rules are ALWAYS ACTIVE for native type serializers and validator components interfacing with Python runtime objects, and modules constructing or traversing Python dictionary structures across language boundaries.

### Rules

- **R-PYO3-001** MUST_NOT: Modules MUST_NOT invoke raw Python C API functions directly when equivalent high-level PyO3 abstractions exist in the resolved PyO3 crate.

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