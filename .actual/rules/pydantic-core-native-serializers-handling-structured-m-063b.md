# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Native Serializers Handling Structured Mapping Data

These rules are ALWAYS ACTIVE for native type serializers and validator components interfacing with the Python runtime, modules responsible for serialization state management, dictionary creation, and string interning, and components maintaining references to runtime Python objects requiring garbage collector traversal.

### Rules

- **R-PYO3-001** MUST: Native serializers handling structured mapping data MUST interface directly through pyo3::types::PyDict to construct and extract key-value pairs without intermediate serialization layers.

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
Claude Code MUST NOT skip or defer verification. All native serializers and validators must adhere strictly to PyO3 runtime bindings, PyDict construction, and explicit cyclic garbage collection traversal protocols.
</enforcement>