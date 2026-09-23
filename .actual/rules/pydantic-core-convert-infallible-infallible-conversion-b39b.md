# Standardization on std::convert::Infallible for Infallible Conversion Contracts: Internal Conversion Contracts That Are Logically

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONV-001** MUST_NOT: Internal conversion contracts that are logically incapable of failing MUST NOT introduce custom uninhabited error types or wrap results in fallible domain error enums.

### Verify

```bash
# Discover and run the project static analysis and compilation check directives defined in the repository configuration.
# Execute the primary project test suite according to the repository test configuration to confirm type checking and conversion behavior.
```

**Accept when:**
- All conversion trait implementations for infallible operations compile successfully with std::convert::Infallible as their associated error type.
- Repository static analysis and test validation suites pass without type mismatch errors or unhandled error warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration build checks, compiler type validation, and peer code reviews.
</enforcement>