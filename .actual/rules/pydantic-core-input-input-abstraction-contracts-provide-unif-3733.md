# LookupPath Module Adoption for Input Traversal and Key Resolution: Input Abstraction Contracts Provide Uniform Error

These rules are ALWAYS ACTIVE for input processing layers, input abstraction definitions, trait contracts, and concrete input representation modules.

### Rules

- **R-LKP-001** SHOULD: Input abstraction contracts SHOULD provide uniform error propagation when a LookupPath lookup yields missing or inaccessible targets.
- **R-LKP-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LKP-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding steps in order (dependency manifest inspection, build tool identification, authoritative lock/resolution artifact inspection, official documentation lookup, API existence confirmation, and per-dependency re-evaluation at point of use).
- **R-LKP-004** MANDATORY: Concrete input types MUST handle both scalar keys and composite paths through the unified LookupPath resolution interface.
- **R-LKP-005** MANDATORY: Input handlers MUST preserve lookup failure semantics consistently so downstream validation layers receive standardized path missing signals.

### Verify

```bash
# Discover and execute the project test runner to validate input traversal test suites
# Run the project static analysis and linting verification suite to ensure compliance with LookupPath interface contracts
```

**Accept when:**
- All input processing modules route key and nested attribute resolution through LookupPath contracts.
- Project verification suites pass without regression in input lookup or validation behavior.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suite execution, continuous integration verification checks, and peer code reviews enforce compliance.
</enforcement>