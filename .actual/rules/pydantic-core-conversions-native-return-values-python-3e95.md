# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Conversions Native Return Values Python Runtime

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PYO3-001** SHOULD: Conversions of native return values to Python runtime objects SHOULD use `pyo3::IntoPyObjectExt` to minimize conversion overhead and maintain ownership transfer semantics.

### Verify

```bash
eval "${DISCOVERED_TEST_COMMAND:?Please set to project discovered test command}"
eval "${DISCOVERED_LINT_COMMAND:?Please set to project discovered lint command}"
```

**Accept when:**
- All native serializer and validator modules compile cleanly and pass full automated test suites with active interpreter bindings.
- Memory leak detection suites confirm zero cyclic memory retention across repeated serialization cycles with complex objects.
- Static analysis checks verify that all repetitive field identifier lookups utilize interned string abstractions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>