# Internal Module Adoption: crate::serializers::SerializationState: Type Serializers Not Mutate Runtime State

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SER-001** MUST_NOT: Type serializers MUST NOT mutate runtime state outside the encapsulation guarantees provided by crate::serializers::SerializationState.

### Verify

```bash
# Discover the project's primary test runner from repository configuration and execute the complete test suite covering the serialization subsystem.
# Discover the project's static analysis and linting entry point from repository metadata and run type verification on all serializer implementations.
```

**Accept when:**
- All serializer test suites pass without regressions across nested and recursive serialization test cases.
- Type checking and static verification pass with zero warnings across all serializer module definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All serializer implementations must be verified through the repository's test runner and static analysis tools.
</enforcement>