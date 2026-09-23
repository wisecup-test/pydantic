# weakref Module Adoption for Metaclass Namespace Caching and Reference Management: Prior Implementing Updating Dependencies That Interact

These rules are ALWAYS ACTIVE for metaclass construction routines capturing parent scopes, caller frame namespaces, and internal reflection utilities or dictionary wrappers caching type resolution metadata.

### Rules

- **R-WEAKREF-001** MUST: Prior to implementing or updating dependencies that interact with runtime reference lifecycles, developers MUST consult the repository dependency manifest and authoritative lock resolution artifact to verify resolved environment constraints.

### Verify

```bash
# Discover and run the project static analysis suite to verify weakref usage across metaclass construction and utility modules.
# Discover and execute the project test runner targeting model construction and namespace resolution test suites to verify garbage collection behavior.
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding reference cycles or improper namespace caching.
- Automated test suites pass, verifying that dynamic model creation releases parent frame references without memory leakage.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code reviews enforce compliance; violations fail automated reviews and block merging.
</enforcement>