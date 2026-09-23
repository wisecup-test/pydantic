# Adoption of serde::ser::Error for Serializer Pipeline Error Handling: Custom Serialization Routines Construct Errors Method

These rules are ALWAYS ACTIVE for all serialization pipeline modules, type serializer implementations, and configuration/context state structures that handle serialization parameters and sentinel values.

### Rules

- **R-SER-001** MUST: Custom serialization routines MUST construct serialization errors using the custom method of the serde::ser::Error trait when translating internal or domain-specific failure conditions.

### Verify

```bash
# Discover and run the project test runner script covering the serialization subsystem
# Discover and run static analysis and linting checks to confirm all serializer error emissions satisfy the error trait contract
cargo test --workspace
cargo clippy --all-targets --all-features
```

**Accept when:**
- All unit and integration tests covering serializer components pass without error contract violations.
- Static type checking and compiler verification succeed without warnings regarding error trait satisfaction.
- Serialization failure test cases verify that custom errors correctly propagate through the serialization pipeline.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing custom serializer errors that fail to implement the framework error trait will be blocked from merging.
</enforcement>