# Internal Module Adoption: crate::serializers::SerializationState: Type Serializer Implementations Accept Coordinate Execution

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SER-001** MUST: Type serializer implementations MUST accept and coordinate execution context through crate::serializers::SerializationState rather than defining disparate parameter signatures for serialization state.

### Verify

```bash
# Discover the project's primary test runner from repository configuration and execute the complete test suite covering the serialization subsystem.
# Discover the project's static analysis and linting entry point from repository metadata and run type verification on all serializer implementations.
```

**Accept when:**
- All serializer test suites pass without regressions across nested and recursive serialization test cases.
- Type checking and static verification pass with zero warnings across all serializer module definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>