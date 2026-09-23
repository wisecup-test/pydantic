# Adoption of serde::ser::Error for Serializer Pipeline Error Handling: Serializer Adapters Wrap Foreign Runtime Exceptions

These rules are ALWAYS ACTIVE for all serialization pipeline modules and type serializer implementations that emit or propagate errors during data conversion.

### Rules

- **R-SER-001** MAY: Serializer adapters MAY wrap foreign runtime exceptions and schema errors into structured messages before passing them to the serde::ser::Error constructor.

### Verify

```bash
# Discover the project test runner script from the repository configuration and execute the complete test suite covering the serialization subsystem.
# Discover and run the project static analysis and linting checks to confirm all serializer error emissions satisfy the error trait contract.
```

**Accept when:**
- All unit and integration tests covering serializer components pass without error contract violations.
- Static type checking and compiler verification succeed without warnings regarding error trait satisfaction.
- Serialization failure test cases verify that custom errors correctly propagate through the serialization pipeline.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying serializer or error handling modules are verified by automated continuous integration test suites, static analysis passes, and peer code reviews.
</enforcement>