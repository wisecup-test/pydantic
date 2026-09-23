# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Native String Slice Representations Referencing Underlying

These rules are ALWAYS ACTIVE for all native type serializers, validators, and Python runtime interop components.

### Rules

- **R-PYO3-001** SHOULD: Native string slice representations referencing underlying Python string memory SHOULD use pyo3::pybacked::PyBackedStr to avoid redundant string copying across the runtime boundary.

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
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests omitting required garbage collection traversal or misusing raw interpreter references are blocked from merging.
</enforcement>