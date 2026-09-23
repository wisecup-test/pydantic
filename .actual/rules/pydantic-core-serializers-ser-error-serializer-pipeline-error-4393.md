# Adoption of serde::ser::Error for Serializer Pipeline Error Handling: Before Implementing Updating Any Component That

These rules are ALWAYS ACTIVE for all serialization pipeline modules, type serializer implementations, configuration, and context state structures that handle serialization parameters and sentinel values.

### Rules

- **R-SER-001** MUST: Before implementing or updating any component that depends on the serialization framework, the consumer MUST inspect the repository dependency manifest and resolution artifacts to confirm the exact resolved version of the serialization library.
- **R-SER-002** MUST: When defining new serializer components, implement error creation methods that invoke the custom constructor provided by serde::ser::Error to wrap internal error messages.
- **R-SER-003** MUST: Ensure that context and state wrappers provide helper utilities for generating standardized serialization error messages before delegating to serializer routines.

### Verify

```bash
# Discover and execute the project test runner script covering the serialization subsystem
# Discover and run the project static analysis and linting checks to confirm all serializer error emissions satisfy the error trait contract
```

**Accept when:**
- All unit and integration tests covering serializer components pass without error contract violations.
- Static type checking and compiler verification succeed without warnings regarding error trait satisfaction.
- Serialization failure test cases verify that custom errors correctly propagate through the serialization pipeline.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites, static analysis passes, and peer code reviews verify compliance; pull requests containing custom serializer errors that fail to implement the framework error trait will be blocked from merging.
</enforcement>