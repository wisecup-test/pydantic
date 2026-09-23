# Adoption of strum::EnumMessage for Enumeration Variant Metadata: Before Introducing Updating Strum Dependencies Consumer

These rules are ALWAYS ACTIVE for input processing and validation enumeration types that expose variant-level descriptions or error messages, and public contract boundaries where enum variants correspond to structured error messages or argument types.

### Rules

- **R-STRUM-001** MUST: Before introducing or updating strum dependencies, the consumer MUST locate the repository dependency manifest and lock artifact to resolve the exact locked version and verify API compatibility against the official version documentation.
- **R-STRUM-002** MUST: Place strum::EnumMessage derive attributes directly on the enumeration declaration along with standard derive attributes.
- **R-STRUM-003** MUST: Access messages via the methods provided by the strum::EnumMessage trait rather than ad-hoc string slicing.

### Verify

```bash
# Discover and execute the project's test suite to verify enum message extraction
cargo test
# Run the project's static analysis and linting scripts
cargo clippy --all-targets --all-features
```

**Accept when:**
- All enumerations requiring descriptive messages derive strum::EnumMessage without manual match dispatch routines.
- Project test suites and compilation checks pass cleanly with zero warnings or errors regarding enum message resolution.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration and peer code reviews enforce these requirements.
</enforcement>