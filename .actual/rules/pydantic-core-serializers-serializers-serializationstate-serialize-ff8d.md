# Standardization on crate::serializers::SerializationState for Serializer Context Management: Type Serializers Minimize Heap Allocations Reusing

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SERIALIZERS-001** SHOULD: Type serializers SHOULD minimize heap allocations by reusing borrows and shared structures within crate::serializers::SerializationState rather than duplicating context data.

### Verify

```bash
# Discover the test runner from the repository configuration and execute unit and integration suites targeting serialization modules.
# Discover the repository static analysis and linting verification tasks to ensure all serializer signatures adhere to state passing rules.
```

**Accept when:**
- All serializer test suites execute successfully with no regression in serialization performance or correctness.
- Static analysis verification completes with zero warnings regarding inconsistent serialization state arguments.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>