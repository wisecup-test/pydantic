# Adoption of importlib for Dynamic Module Loading and Deferred Export Resolution: Developers Inspect Repository Dependency Manifest Authoritative

These rules are ALWAYS ACTIVE for package root entry points, runtime execution harnesses, and migration layers requiring dynamic module loading and deferred export resolution.

### Rules

- **R-IM-001** MUST: Developers MUST inspect the repository dependency manifest and authoritative lock artifact to resolve exact dependency versions prior to implementing dynamic module loading routines.

### Verify

```bash
# Discover and execute the repository test runner suite to verify that dynamic module exports resolve successfully without import cycles or startup failures.
# Run the repository static analysis and type verification routines to ensure dynamic import definitions satisfy public interface contracts.
```

**Accept when:**
- All dynamic module lookups resolve attributes correctly at runtime without triggering premature initialization during module import.
- Automated test suites pass without circular import errors across target runtime execution environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test suites and code review checks.
</enforcement>