# weakref Module Adoption for Metaclass Namespace Caching and Reference Management: Subclass Registration Namespace Interception Hooks That

These rules are ALWAYS ACTIVE for all metaclass construction routines, subclass registration/namespace interception hooks, and internal reflection utilities capturing parent scopes or caller frame namespaces.

### Rules

- **R-WEAKREF-001** MUST: Subclass registration and namespace interception hooks that monitor decorator overrides or attribute assignments MUST maintain decoupled reference graphs that do not block garbage collection of target descriptors.
- **R-WEAKREF-002** MUST: Follow the LOCK-VERSION GROUNDING procedure before writing code that uses a versioned library: find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm APIs exist, and re-run per dependency at point of use.
- **R-WEAKREF-003** MUST: Construct weak reference dictionaries using specialized helpers that gracefully tolerate un-weakreferenceable objects by ignoring or handling them leniently.
- **R-WEAKREF-004** MUST: Ensure all namespace resolver components unpack weak-value mappings immediately prior to attribute inspection to minimize stale reference windows.

### Verify

```bash
# Discover and run the project static analysis suite to verify weakref usage across metaclass construction and utility modules.
# Discover and execute the project test runner targeting model construction and namespace resolution test suites to verify garbage collection behavior.
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding reference cycles or improper namespace caching.
- Automated test suites pass, verifying that dynamic model creation releases parent frame references without memory leakage.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, mandatory peer code review, and automated memory regression test suites. Violations will fail automated reviews and block merging.
</enforcement>