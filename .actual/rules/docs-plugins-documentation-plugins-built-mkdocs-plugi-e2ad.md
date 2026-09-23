# mkdocs.plugin Lifecycle Hook Runtime Environment Configuration Management: Documentation Plugins Built Mkdocs Plugin Lifecycle

These rules are ALWAYS ACTIVE for documentation plugins implementing lifecycle hook contracts and build extensions requiring dynamic execution flags or external service credentials.

### Rules

- **R-DOC-001** MUST: Documentation plugins built on mkdocs.plugin lifecycle hooks MUST source dynamic execution toggles and external integration credentials directly from runtime environment variables rather than persisting them in repository configuration files.

### Verify

```bash
# Discover and run the project test suite to verify documentation plugin lifecycle hooks
# Discover and execute the documentation build verification procedure under clean environment settings
```

**Accept when:**
- Documentation build lifecycle hooks execute successfully when external credentials and environment flags are omitted in local environments.
- Execution paths requiring external integration tokens abort with descriptive plugin errors when mandatory environment variables are missing during deployment builds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites executing plugin lifecycle test cases and peer code review.
</enforcement>