# Adoption of importlib for Dynamic Module Loading and Deferred Export Resolution: Attribute Migration Wrappers That Invoke Importlib

These rules are ALWAYS ACTIVE for package root entry points providing public attribute exports, migration paths, and runtime execution harnesses/test runners requiring dynamic module loading.

### Rules

- **R-IMP-001** SHOULD: Attribute migration wrappers that invoke importlib SHOULD trigger deprecation warnings when resolving legacy or relocated public attributes.

### Verify

```bash
# Discover and execute the repository test runner suite to verify dynamic module exports
# Run the repository static analysis and type verification routines
```

**Accept when:**
- All dynamic module lookups resolve attributes correctly at runtime without triggering premature initialization during module import.
- Automated test suites pass without circular import errors across target runtime execution environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test suites and code review checks.
</enforcement>