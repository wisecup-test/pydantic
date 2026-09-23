# DefinitionsBuilder Internal Module Adoption for Serialization Type Construction: Type Serializers That Support Recursive Shared

These rules are ALWAYS ACTIVE for all type serializer implementations and computed field serializer components participating in schema compilation, constructors, and builder routines responsible for assembling serialization logic from schema definitions.

### Rules

- **R-SERIALIZER-001** MUST: Type serializers that support recursive or shared definition references MUST register and query definition identifiers through DefinitionsBuilder rather than managing standalone definition registries.
- **R-SERIALIZER-002** MUST: When implementing a new serializer builder, accept DefinitionsBuilder mutably and register any declared definition identifier before compiling child schemas.
- **R-SERIALIZER-003** MUST: Wrap shared child serializers in thread-safe reference-counted pointers to allow multi-threaded reuse of immutable serializer graphs.

### Verify

```bash
# Discover the project test runner from the root manifest and run the serialization test suite to verify type serializer registration
# Discover the static analysis and linter configurations from the repository and run all verification checks against the serializer modules
```

**Accept when:**
- All serializer construction routines correctly pass DefinitionsBuilder and compile without missing reference errors.
- The discovered test suite passes all serialization test cases for all registered type serializers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via static type analysis, compiler checks, automated test suites exercising recursive and shared definition serialization paths, and peer review on pull requests touching serializer construction modules.
</enforcement>