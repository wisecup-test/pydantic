# DefinitionsBuilder Internal Module Adoption for Serialization Type Construction: Before Compiling Executing Code External Internal

These rules are ALWAYS ACTIVE for all type serializer implementations and computed field serializer components participating in schema compilation, as well as constructors and builder routines responsible for assembling serialization logic from schema definitions.

### Rules

- **R-DEF-001** MUST: Before compiling or executing code with external or internal module dependencies, engineers MUST inspect the project lock artifact to verify resolved dependency versions and validate that referenced APIs exist in the authoritative documentation.
- **R-DEF-002** MUST: When implementing a new serializer builder, accept DefinitionsBuilder mutably and register any declared definition identifier before compiling child schemas.
- **R-DEF-003** MUST: Wrap shared child serializers in thread-safe reference-counted pointers to allow multi-threaded reuse of immutable serializer graphs.

### Verify

```bash
# Discover the project test runner from the root manifest and run the serialization test suite to verify type serializer registration.
# Discover the static analysis and linter configurations from the repository and run all verification checks against the serializer modules.
```

**Accept when:**
- All serializer construction routines correctly pass DefinitionsBuilder and compile without missing reference errors.
- The discovered test suite passes all serialization test cases for all registered type serializers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static type analysis, compiler checks, and automated test suites exercising recursive and shared definition serialization paths.
</enforcement>