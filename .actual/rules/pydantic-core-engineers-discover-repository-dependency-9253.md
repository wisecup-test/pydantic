# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Engineers Discover Repository Dependency Manifest Inspect

These rules are ALWAYS ACTIVE for native type serializers, validators, and modules interfacing with the Python runtime.

### Rules

- **R-PYO3-001** MUST: Engineers MUST discover the repository dependency manifest and inspect the authoritative repository lock artifact to determine and verify the exact resolved version of the foreign function interface library prior to implementing or modifying runtime bindings.
- **R-PYO3-002** MUST: Verify that all new type serializers handling mapping objects construct outputs via pyo3::types::PyDict interfaces and leverage pyo3::intern for repeated field names.
- **R-PYO3-003** MUST: Ensure any custom serializer holding references to Python objects implements cycle traversal to satisfy garbage collection requirements.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration test suites, peer code review, and static linting enforcing trait implementations and lifetime bindings.
</enforcement>