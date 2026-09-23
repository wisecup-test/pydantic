# Adoption of pydantic.v1 Internal Compatibility Module Namespace: Compatibility Model Configurations Within Internal Namespace

These rules are ALWAYS ACTIVE for internal modules implementing legacy validation models, argument decorators, network parsing primitives, and datetime parsing routines within the compatibility module namespace.

### Rules

- **R-PVP-001** SHOULD: Compatibility model configurations within the internal namespace SHOULD subclass BaseConfig or CustomConfig while explicitly defining extra field handling behaviors.

### Verify

```bash
# Discover the project test execution runner from repository build manifests and run the compatibility test suite targeting the legacy module namespace.
# Discover the project static analysis and linting configuration from the repository and execute import boundary validation to detect unauthorized imports from the legacy module namespace.
```

**Accept when:**
- All automated test suites exercising legacy validation decorators, network data structures, and datetime parsing routines pass without error.
- Static analysis verifies zero unintended cross-boundary import couplings from the compatibility module namespace into modern core namespaces.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test matrices, static analysis linters enforcing import boundaries, and peer code review verification.
</enforcement>