# Standardization on collections.abc for Container Protocols and Mapping Implementations: Implementations Collections Abc Mapping Evaluate Container

These rules are ALWAYS ACTIVE for internal library modules authoring custom namespace, dictionary, or sequence data structures, and subsystem boundaries defining structural container protocols and type contracts.

### Rules

- **R-COL-001** MAY: Implementations of collections.abc.Mapping MAY evaluate container contents lazily using cached properties provided that protocol invariants are preserved.
- **R-COL-002** MANDATORY: When subclassing collections.abc.Mapping, developers must provide implementations for __getitem__, __len__, and __iter__.
- **R-COL-003** MANDATORY: Before writing code that uses a versioned library, consumers MUST execute the lock-version grounding process (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for exact version, confirm API existence, and re-run per dependency at point of use).

### Verify

```bash
bash -c 'DISCOVERED_CHECKER=$(command -v "${PROJECT_TYPE_CHECKER:-}" || true); if [ -n "$DISCOVERED_CHECKER" ]; then "$DISCOVERED_CHECKER"; else echo "Discover and run the project type checking script from the repository manifest"; fi'
bash -c 'DISCOVERED_TEST_RUNNER=$(command -v "${PROJECT_TEST_RUNNER:-}" || true); if [ -n "$DISCOVERED_TEST_RUNNER" ]; then "$DISCOVERED_TEST_RUNNER"; else echo "Discover and run the project test suite from the repository manifest"; fi'
```

**Accept when:**
- All custom container and namespace classes inherit from collections.abc abstract base classes and pass protocol validation tests.
- Verification suites confirm that static type checking and runtime instantiation succeed without abstract method errors.
- Lock artifact version grounding has been performed and verified against the repository configuration.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, type checking, and peer code review verification against abstract container protocol rules are mandatory.
</enforcement>