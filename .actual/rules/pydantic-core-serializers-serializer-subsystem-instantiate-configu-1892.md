# DefinitionsBuilder Internal Module Adoption for Serialization Type Construction: Serializer Subsystem Instantiate Configure Type Serializers

These rules are ALWAYS ACTIVE for all type serializer implementations, computed field serializer components participating in schema compilation, and constructors/builder routines responsible for assembling serialization logic from schema definitions.

### Rules

- **R-SER-001** MUST: The serializer subsystem MUST instantiate and configure type serializers exclusively through DefinitionsBuilder to maintain unified definition registration and reference resolution.
- **R-SER-002** MANDATORY (Discovery Policy): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository and omit hardcoded placeholders.
- **R-SER-003** MANDATORY (Lock-Version Grounding): Before writing code using a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-verify per dependency at point of use.
- **R-SER-004** MUST: When implementing a new serializer builder, accept DefinitionsBuilder mutably and register any declared definition identifier before compiling child schemas.
- **R-SER-005** MUST: Wrap shared child serializers in thread-safe reference-counted pointers to allow multi-threaded reuse of immutable serializer graphs.

### Verify

```bash
# Discover the project test runner from the root manifest and run the serialization test suite
# Discover the static analysis and linter configurations from the repository and run all verification checks against the serializer modules
```

**Accept when:**
- All serializer construction routines correctly pass DefinitionsBuilder and compile without missing reference errors.
- The discovered test suite passes all serialization test cases for all registered type serializers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static type analysis, compiler checks, and automated test suites exercising recursive and shared definition serialization paths.
</enforcement>