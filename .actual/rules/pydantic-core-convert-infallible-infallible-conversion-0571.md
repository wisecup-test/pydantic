# Standardization on std::convert::Infallible for Infallible Conversion Contracts: Before Integrating Updating Dependency Interfaces Consumer

These rules are ALWAYS ACTIVE for trait implementations, conversion adapters, and abstract input interfaces within core processing modules where operations cannot fail.

### Rules

- **R-CON-001** MUST: Before integrating or updating dependency interfaces, the consumer MUST discover the project dependency lock file and verify resolved versions against the manifest.
- **R-CON-002** MUST: When implementing conversion traits for types guaranteed to convert without error, declare the associated error type as std::convert::Infallible.
- **R-CON-003** MUST: Ensure downstream pattern match expressions take advantage of the uninhabited nature of the error type to eliminate runtime checks.

### Verify

```bash
# Discover and run the project static analysis and compilation check directives defined in the repository configuration.
# Execute the primary project test suite according to the repository test configuration to confirm type checking and conversion behavior.
```

**Accept when:**
- All conversion trait implementations for infallible operations compile successfully with std::convert::Infallible as their associated error type.
- Repository static analysis and test validation suites pass without type mismatch errors or unhandled error warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration build checks and peer review.
</enforcement>