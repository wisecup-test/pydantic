# Adoption of pydantic.v1 Internal Compatibility Module Namespace: Modules Outside Legacy Compatibility Boundary Not

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PYD-001** MUST_NOT: Modules outside the legacy compatibility boundary MUST NOT import deprecated validation constructs directly when contemporary replacement contracts are available in root module namespaces.

### Verify

```bash
# Discover the project test execution runner from repository build manifests and run the compatibility test suite targeting the legacy module namespace.
# Discover the project static analysis and linting configuration from the repository and execute import boundary validation to detect unauthorized imports from the legacy module namespace.
```

**Accept when:**
- All automated test suites exercising legacy validation decorators, network data structures, and datetime parsing routines pass without error.
- Static analysis verifies zero unintended cross-boundary import couplings from the compatibility module namespace into modern core namespaces.

<enforcement>
Claude Code MUST NOT skip or defer verification.\
n</enforcement>