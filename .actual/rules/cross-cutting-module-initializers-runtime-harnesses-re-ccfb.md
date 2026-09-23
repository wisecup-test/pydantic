# Adoption of importlib for Dynamic Module Loading and Deferred Export Resolution: Module Initializers Runtime Harnesses Requiring Dynamic

These rules are ALWAYS ACTIVE for all package root entry points, runtime execution harnesses, and test runners requiring dynamic module loading or deferred export resolution.

### Rules

- **R-MOD-001** MUST: Module initializers and runtime harnesses requiring dynamic export resolution or runtime module execution MUST use programmatic module loading via the importlib module instead of eager top-level static imports.

### Verify

```bash
# Discover and execute the repository test runner suite to verify dynamic module exports
# Run the repository static analysis and type verification routines
```

**Accept when:**
- All dynamic module lookups resolve attributes correctly at runtime without triggering premature initialization during module import.
- Automated test suites pass without circular import errors across target runtime execution environments.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>