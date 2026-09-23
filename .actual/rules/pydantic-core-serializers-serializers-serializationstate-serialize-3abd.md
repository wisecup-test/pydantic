# Standardization on crate::serializers::SerializationState for Serializer Context Management: Serializer Implementations Not Introduce Disjoint Hoc

These rules are ALWAYS ACTIVE for all schema serializers, prebuilt serializers, and type serializers within the serialization subsystem.

### Rules

- **R-SER-001** MUST_NOT: Serializer implementations MUST NOT introduce disjoint, ad-hoc state objects or thread-local storage for state data that belongs to the active serialization pass.

### Verify

```bash
# Discover the test runner from the repository configuration and execute unit and integration suites targeting serialization modules.
# Discover the repository static analysis and linting verification tasks to ensure all serializer signatures adhere to state passing rules.
```

**Accept when:**
- All serializer test suites execute successfully with no regression in serialization performance or correctness.
- Static analysis verification completes with zero warnings regarding inconsistent serialization state arguments.

<enforcement>
Claude Code MUST NOT skip or defer verification. All serializer implementations must thread crate::serializers::SerializationState as an active parameter and pass it into child serializer calls.
</enforcement>