# LookupPath Module Adoption for Input Traversal and Key Resolution: Implementations Provide Optimized Fast Paths Single

These rules are ALWAYS ACTIVE for all input processing layers, input abstraction definitions, and concrete input representation modules.

### Rules

- **R-LOOKUP-001** MAY: Implementations MAY provide optimized fast paths for single-segment keys within the LookupPath contract boundaries.
- **R-LOOKUP-002** MANDATORY: Input processing modules MUST route key and nested attribute resolution through LookupPath contracts.
- **R-LOOKUP-003** MANDATORY: Concrete input types MUST handle both scalar keys and composite paths through the unified LookupPath resolution interface.
- **R-LOOKUP-004** MANDATORY: Lookup failure semantics MUST be preserved consistently so downstream validation layers receive standardized path missing signals.
- **R-LOOKUP-005** MANDATORY: Prior to writing code that uses a versioned library, developers MUST discover the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for exact versions, check official documentation for that exact version, confirm APIs exist, and re-verify per dependency at point of use.

### Verify

```bash
# Discover and execute the project test runner to validate input traversal test suites.
# Run the project static analysis and linting verification suite to ensure compliance with LookupPath interface contracts.
```

**Accept when:**
- All input processing modules route key and nested attribute resolution through LookupPath contracts.
- Project verification suites pass without regression in input lookup or validation behavior.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suite execution, continuous integration verification checks, and peer code reviews validate that new input types implement LookupPath traversal contracts.
</enforcement>