# DefinitionsBuilder Internal Module Adoption for Serialization Type Construction: Type Serializer Constructors Requiring Shared Ownership

These rules are ALWAYS ACTIVE for all type serializer implementations and computed field serializer components participating in schema compilation.

### Rules

- **R-SER-001** MUST: Type serializer constructors requiring shared ownership of compiled sub-serializers MUST store resolved definitions behind thread-safe reference-counted pointers.

### Verify

```bash
# Discover the project test runner from the root manifest and run the serialization test suite to verify type serializer registration.
# Discover the static analysis and linter configurations from the repository and run all verification checks against the serializer modules.
```

**Accept when:**
- All serializer construction routines correctly pass DefinitionsBuilder and compile without missing reference errors.
- The discovered test suite passes all serialization test cases for all registered type serializers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>