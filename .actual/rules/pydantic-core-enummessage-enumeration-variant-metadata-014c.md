# Adoption of strum::EnumMessage for Enumeration Variant Metadata: Variant Messages Defined Under Strum Enummessage

These rules are ALWAYS ACTIVE for input processing and validation enumeration types that expose variant-level descriptions or error messages.

### Rules

- **R-ENUM-001** SHOULD: Variant messages defined under strum::EnumMessage SHOULD provide concise, human-readable descriptions that remain consistent across validation and serialization boundaries.

### Verify

```bash
# Discover and execute the project's test suite to verify enum message extraction
cargo test

# Discover and run static analysis/linting
cargo clippy --all-targets --all-features
```

**Accept when:**
- All enumerations requiring descriptive messages derive strum::EnumMessage without manual match dispatch routines.
- Project test suites and compilation checks pass cleanly with zero warnings or errors regarding enum message resolution.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>