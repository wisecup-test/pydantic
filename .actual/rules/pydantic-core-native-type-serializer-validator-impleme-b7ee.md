# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Native Type Serializer Validator Implementations Adopt

These rules are ALWAYS ACTIVE for all native type serializers and validators interfacing with the Python runtime, modules responsible for serialization state management, dictionary creation, and string interning, and components maintaining references to runtime Python objects requiring garbage collector traversal.

### Rules

- **R-PYO3-001** MUST: Native type serializer and validator implementations MUST adopt the PyO3 library for foreign function interface interoperability and Python runtime type bindings.
- **R-PYO3-002** MUST: All new type serializers handling mapping objects construct outputs via `pyo3::types::PyDict` interfaces and leverage `pyo3::intern` for repeated field names.
- **R-PYO3-003** MUST: Any custom serializer holding references to Python objects implements cycle traversal to satisfy garbage collection requirements.

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