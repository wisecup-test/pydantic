#
 Adoption of pydantic.v1 Internal Compatibility Module Namespace: Network Address Uniform Resource Identifier Representations

These rules are ALWAYS ACTIVE for all code implementing legacy validation models, argument decorators, network parsing primitives, and datetime parsing routines requiring the `pydantic.v1` compatibility namespace.

### Rules

- **R-NET-001** MUST: Network address and uniform resource identifier representations requiring schema validation MUST inherit from AnyUrl or implement the Parts and HostParts interface contracts within the pydantic.v1 namespace.

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