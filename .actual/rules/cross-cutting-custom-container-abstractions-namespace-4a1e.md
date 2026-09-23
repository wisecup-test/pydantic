# Standardization on collections.abc for Container Protocols and Mapping Implementations: Custom Container Abstractions Namespace Structures Implementing

These rules are ALWAYS ACTIVE for internal library modules authoring custom namespace, dictionary, or sequence data structures, and subsystem boundaries defining structural container protocols and type contracts.

### Rules

- **R-CONTAINER-001** MUST: All custom container abstractions and namespace structures implementing mapping semantics MUST inherit directly from collections.abc.Mapping and implement the full protocol contract (__getitem__, __len__, and __iter__). 
- **R-CONTAINER-002** MUST: Execute lock-version grounding (find manifest, identify build tool, inspect lock artifact, look up official docs, confirm APIs) before writing code that uses a versioned library.
- **R-CONTAINER-003** MUST: Adhere to the Discovery Policy: omit all tool names, file names, commands, package managers, and version numbers, deriving them from the project repository.

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