# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Serializers Holding Native References Runtime Python

These rules are ALWAYS ACTIVE for all native type serializers and validator components interfacing with the Python runtime, modules responsible for serialization state management, and components maintaining references to runtime Python objects requiring garbage collector traversal.

### Rules

- **R-PYO3-001** MUST: Serializers holding native references to runtime Python objects that participate in cyclic reference graphs MUST implement garbage collection traversal using pyo3::gc::PyVisit and return pyo3::PyTraverseError upon traversal failure.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration test suites, peer code review, and static linting/compiler checks.
</enforcement>