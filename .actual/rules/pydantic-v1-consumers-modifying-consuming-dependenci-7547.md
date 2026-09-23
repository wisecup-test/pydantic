# Adoption of pydantic.v1 Internal Compatibility Module Namespace: Consumers Modifying Consuming Dependencies Referenced Compatibility

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PYD-001** MUST: Consumers modifying or consuming dependencies referenced by the compatibility layer MUST inspect the repository lock artifact to verify the exact resolved version before integrating interface signatures.

### Verify

```bash
# Discover the project test execution runner from repository build manifests and run the compatibility test suite targeting the legacy module namespace.
# Discover the project static analysis and linting configuration from the repository and execute import boundary validation to detect unauthorized imports from the legacy module namespace.
```

**Accept when:**
- All automated test suites exercising legacy validation decorators, network data structures, and datetime parsing routines pass without error.
- Static analysis verifies zero unintended cross-boundary import couplings from the compatibility module namespace into modern core namespaces.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>