# DefinitionsBuilder Internal Module Adoption for Serialization Type Construction: Serializer Builder Implementations Pass Definitionsbuilder References

These rules are ALWAYS ACTIVE for all type serializer implementations, computed field serializer components participating in schema compilation, and constructors/builder routines responsible for assembling serialization logic from schema definitions.

### Rules

- **R-SER-001** SHOULD: Serializer builder implementations SHOULD pass DefinitionsBuilder references mutably across constructor hierarchies to guarantee consistent deferred definition resolution.
- **R-SER-002** MANDATORY: When implementing a new serializer builder, accept DefinitionsBuilder mutably and register any declared definition identifier before compiling child schemas.
- **R-SER-003** MANDATORY: Wrap shared child serializers in thread-safe reference-counted pointers to allow multi-threaded reuse of immutable serializer graphs.

### Verify

```bash
# Discover the project test runner from the root manifest and run the serialization test suite to verify type serializer registration.
# Discover the static analysis and linter configurations from the repository and run all verification checks against the serializer modules.
```

**Accept when:**
- All serializer construction routines correctly pass DefinitionsBuilder and compile without missing reference errors.
- The discovered test suite passes all serialization test cases for all registered type serializers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified via static type analysis, compiler checks, automated test suites exercising recursive and shared definition paths, and peer review.
</enforcement>