# Internal Module Adoption: crate::serializers::SerializationState: When Encountering Unexpected Payload Types During

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SERIALIZERS-001** SHOULD: When encountering unexpected payload types during serialization traversal, serializers SHOULD raise crate::PydanticSerializationUnexpectedValue to preserve uniform error telemetry.

### Verify

```bash
# Discover the project's primary test runner from repository configuration and execute the complete test suite covering the serialization subsystem.
# Discover the project's static analysis and linting entry point from repository metadata and run type verification on all serializer implementations.
```

**Accept when:**
- All serializer test suites pass without regressions across nested and recursive serialization test cases.
- Type checking and static verification pass with zero warnings across all serializer module definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All serializer modifications must adhere to shared state management and uniform error handling.
</enforcement>