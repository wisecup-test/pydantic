# weakref Module Adoption for Metaclass Namespace Caching and Reference Management: Internal Caching Layers Store Transient Type

These rules are ALWAYS ACTIVE for metaclass construction routines, internal reflection utilities, and dictionary wrappers caching type resolution metadata.

### Rules

- **R-WEAKREF-001** MAY: Internal caching layers MAY store transient type evaluation metadata in weak-key or weak-value containers when metadata lifetime must mirror target class lifetime.

### Verify

```bash
# Discover and run the project static analysis suite to verify weakref usage across metaclass construction and utility modules.
# Discover and execute the project test runner targeting model construction and namespace resolution test suites to verify garbage collection behavior.
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding reference cycles or improper namespace caching.
- Automated test suites pass, verifying that dynamic model creation releases parent frame references without memory leakage.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>