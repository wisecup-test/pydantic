# LookupPath Module Adoption for Input Traversal and Key Resolution: Input Processing Abstractions Concrete Handling Types

These rules are ALWAYS ACTIVE for input processing abstractions, concrete input handling types, and related path-based key navigation or element resolution routines.

### Rules

- **R-INP-001** MUST: Input processing abstractions and concrete input handling types MUST use LookupPath for defining and executing path-based key navigation and element resolution.

### Verify

```bash
# Discover and execute the project test runner to validate input traversal test suites.
# Run the project static analysis and linting verification suite to ensure compliance with LookupPath interface contracts.
```

**Accept when:**
- All input processing modules route key and nested attribute resolution through LookupPath contracts.
- Project verification suites pass without regression in input lookup or validation behavior.

<enforcement>
Claude Code MUST NOT skip or defer verification. All input processing modules must route key and nested attribute resolution through LookupPath contracts.
</enforcement>