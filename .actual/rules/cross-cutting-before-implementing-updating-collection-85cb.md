# Standardization on collections.abc for Container Protocols and Mapping Implementations: Before Implementing Updating Collection Interfaces Developers

These rules are ALWAYS ACTIVE for internal library modules authoring custom namespace, dictionary, or sequence data structures and subsystem boundaries defining structural container protocols and type contracts.

### Rules

- **R-COL-001** MUST: Before implementing or updating collection interfaces, developers MUST inspect the repository lock artifact to verify the active runtime environment specifications and library constraints.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>