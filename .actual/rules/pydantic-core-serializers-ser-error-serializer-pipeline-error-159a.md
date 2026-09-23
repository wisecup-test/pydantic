# Adoption of serde::ser::Error for Serializer Pipeline Error Handling: Serializer Modules Not Bypass Serde Ser

These rules are ALWAYS ACTIVE for all serialization pipeline modules and type serializer implementations that emit or propagate errors during data conversion.

### Rules

- **R-SERDE-001** MUST_NOT: Serializer modules MUST_NOT bypass the serde::ser::Error trait by returning raw unformatted strings or foreign error types that fail to implement the serialization framework error contract.

### Verify

```bash
# Discover and run the project test runner script covering the serialization subsystem
# Discover and run static analysis and linting checks to confirm serializer error compliance
```

**Accept when:**
- All unit and integration tests covering serializer components pass without error contract violations.
- Static type checking and compiler verification succeed without warnings regarding error trait satisfaction.
- Serialization failure test cases verify that custom errors correctly propagate through the serialization pipeline.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>