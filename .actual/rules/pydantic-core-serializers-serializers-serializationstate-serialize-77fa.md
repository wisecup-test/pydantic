# Standardization on crate::serializers::SerializationState for Serializer Context Management: Before Writing Code That Interacts External

These rules are ALWAYS ACTIVE for implementation of schema serializers, prebuilt serializers, and type serializers within the serialization subsystem, and type-specific serialization logic requiring traversal state, recursion control, or configuration flags.

### Rules

- **R-SER-001** MUST: Before writing code that interacts with external versioned dependencies supporting serialization, the consumer MUST inspect the project lock artifact to ground implementation against the exact resolved dependency version.
- **R-SER-002** MUST: When writing code that interacts with external versioned dependencies, follow the MANDATORY lock-version grounding process: find the manifest, identify the build tool, inspect the repository lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use for version-sensitive behavior.
- **R-SER-003** MUST: Implement serialization methods to receive `crate::serializers::SerializationState` as an active parameter and pass it into child serializer calls when defining new type serializers.
- **R-SER-004** MUST: Coordinate definitions resolution by integrating `DefinitionsBuilder` during schema preparation to ensure type serializers resolve recursive identifiers through `crate::serializers::SerializationState`.
- **R-SER-005** MUST: Enforce instance-per-root-traversal lifecycles in top-level serializer dispatch entrypoints to prevent accidental state leakage between independent serialization passes.

### Verify

```bash
# Discover the test runner from repository configuration and execute unit and integration suites
# Discover and execute repository static analysis and linting verification tasks
```

**Accept when:**
- All serializer test suites execute successfully with no regression in serialization performance or correctness.
- Static analysis verification completes with zero warnings regarding inconsistent serialization state arguments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and static analysis checks will verify function signature conformance, and pull requests bypassing `crate::serializers::SerializationState` will fail automated checks and review approval.
</enforcement>