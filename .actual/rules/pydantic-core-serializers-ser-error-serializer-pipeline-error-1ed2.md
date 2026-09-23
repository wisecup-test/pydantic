# Adoption of serde::ser::Error for Serializer Pipeline Error Handling: Serialization State Containers Context Wrappers Integrate

These rules are ALWAYS ACTIVE for all serialization pipeline modules, type serializers, configuration, and context state structures that handle serialization parameters and sentinel values.

### Rules

- **R-SER-001** SHOULD: Serialization state containers and context wrappers SHOULD integrate custom error creation helpers that map runtime type mismatches and sentinel omissions directly to serde::ser::Error conforming structures.
- **R-SER-002** MANDATORY: When defining new serializer components, implement error creation methods that invoke the custom constructor provided by serde::ser::Error to wrap internal error messages.
- **R-SER-003** MANDATORY: Ensure that context and state wrappers provide helper utilities for generating standardized serialization error messages before delegating to serializer routines.

### Verify

```bash
# Discover the project test runner script and static analysis tools from the repository configuration, then execute:
# 1. Complete test suite covering the serialization subsystem
# 2. Static analysis and linting checks to confirm serializer error emissions satisfy the error trait contract
```

**Accept when:**
- All unit and integration tests covering serializer components pass without error contract violations.
- Static type checking and compiler verification succeed without warnings regarding error trait satisfaction.
- Serialization failure test cases verify that custom errors correctly propagate through the serialization pipeline.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites and static analysis passes verify these rules. Pull requests containing custom serializer errors that fail to implement the framework error trait will be blocked from merging.
</enforcement>