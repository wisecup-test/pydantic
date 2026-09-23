# weakref Module Adoption for Metaclass Namespace Caching and Reference Management: Metaclass Lifecycle Handlers Not Hold Strong

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-WAK-001** MUST_NOT: Metaclass lifecycle handlers MUST_NOT hold strong references to execution frames, caller globals, or mutable namespace dictionaries across class construction boundaries.

### Verify

```bash
# Discover and run the project static analysis suite to verify weakref usage across metaclass construction and utility modules.
# Discover and execute the project test runner targeting model construction and namespace resolution test suites to verify garbage collection behavior.
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding reference cycles or improper namespace caching.
- Automated test suites pass, verifying that dynamic model creation releases parent frame references without memory leakage.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis checks and test suites must verify weakref usage and reference management across metaclass construction.
</enforcement>