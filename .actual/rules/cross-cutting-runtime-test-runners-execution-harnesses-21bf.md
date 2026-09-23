# Adoption of importlib for Dynamic Module Loading and Deferred Export Resolution: Runtime Test Runners Execution Harnesses Use

These rules are ALWAYS ACTIVE for all runtime test runners, execution harnesses, and package root entry points requiring dynamic module loading.

### Rules

- **R-IMPORT-001** MAY: Runtime test runners and execution harnesses MAY use importlib to configure and load execution components dynamically during runtime initialization.
- **R-IMPORT-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-IMPORT-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact version, (4) Look up official documentation for that exact version without relying solely on training data, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-IMPORT-004** MANDATORY: Implement dynamic attribute resolution by defining a centralized dynamic import map and binding module getattr dispatchers to resolve attributes programmatically, coupling dynamic attribute migrations with standard deprecation warnings.

### Verify

```bash
# Discover and execute the repository test runner suite to verify dynamic module exports
# Run repository static analysis and type verification routines
```

**Accept when:**
- All dynamic module lookups resolve attributes correctly at runtime without triggering premature initialization during module import.
- Automated test suites pass without circular import errors across target runtime execution environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites and code review checks verify package entry import times and module resolution.
</enforcement>