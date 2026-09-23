# collections.abc Mapping Protocol Adoption for Namespace and Configuration Resolution: Custom Mapping Implementations Designed Deferred Evaluation

These rules are ALWAYS ACTIVE for all internal modules defining custom mapping objects for evaluation namespaces and configuration processing routines accepting or merging dictionary-like settings.

### Rules

- **R-MAP-001** MUST: Custom mapping implementations designed for deferred evaluation MUST defer multi-scope dictionary merging until attribute or item retrieval occurs.

### Verify

```bash
# Discover and execute the repository type-checking suite to confirm all custom mapping classes satisfy the collections.abc.Mapping protocol.
# Discover and execute the repository test runner targeting namespace resolution and configuration processing modules.
```

**Accept when:**
- All test suites exercising namespace evaluation and configuration resolution pass without protocol mismatch errors.
- Static type checking passes across all custom collections.abc mapping implementations without protocol omissions.
- Namespace construction benchmarks show deferred evaluation without eager dictionary materialization upon initialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static type analysis and unit test suites.
</enforcement>