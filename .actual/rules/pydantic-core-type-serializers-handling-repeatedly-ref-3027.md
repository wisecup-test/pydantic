# PyO3 Library Adoption for Type Serialization and Python Runtime Interoperability: Type Serializers Handling Repeatedly Referenced Dictionary

These rules are ALWAYS ACTIVE for all native type serializers, validators, and modules interfacing with the Python runtime.

### Rules

- **R-PYO3-001** MUST: Type serializers handling repeatedly referenced dictionary keys and object attribute identifiers MUST use pyo3::intern to cache and reuse interned string references across serialization invocations.

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