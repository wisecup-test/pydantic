# weakref Module Adoption for Metaclass Namespace Caching and Reference Management: Core Class Construction Metaclasses Wrap Captured

These rules are ALWAYS ACTIVE for all core class construction metaclasses, internal reflection utilities, and namespace caching modules.

### Rules

- **R-WREF-001** MUST: Core class construction metaclasses MUST wrap captured parent namespaces in weak reference dictionaries derived from the standard library weakref module rather than storing raw dictionary or frame references.

### Verify

```bash
# Discover and run the project static analysis suite to verify weakref usage across metaclass construction and utility modules.
# Discover and execute the project test runner targeting model construction and namespace resolution test suites to verify garbage collection behavior.
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding reference cycles or improper namespace caching.
- Automated test suites pass, verifying that dynamic model creation releases parent frame references without memory leakage.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and test suite checks are mandatory for all changes touching metaclass construction and reference handling.
</enforcement>