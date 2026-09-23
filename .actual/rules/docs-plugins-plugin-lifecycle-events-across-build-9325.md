# mkdocs.plugin Lifecycle Hook Runtime Environment Configuration Management: Plugin Lifecycle Events Across Build Phases

These rules are ALWAYS ACTIVE for documentation plugins implementing lifecycle hook contracts and build extensions requiring dynamic execution flags or external service credentials.

### Rules

- **R-LIFE-001** SHOULD: Plugin lifecycle events across build phases SHOULD log operational warnings and state transitions through plugin logging instances obtained from logging getLogger.

### Verify

```bash
# Discover and run the project test suite to verify documentation plugin lifecycle hooks handle both present and absent environment variables.
# Execute the documentation build verification procedure under clean environment settings to ensure local preview generation succeeds without external credentials.
```

**Accept when:**
- Documentation build lifecycle hooks execute successfully when external credentials and environment flags are omitted in local environments.
- Execution paths requiring external integration tokens abort with descriptive plugin errors when mandatory environment variables are missing during deployment builds.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>