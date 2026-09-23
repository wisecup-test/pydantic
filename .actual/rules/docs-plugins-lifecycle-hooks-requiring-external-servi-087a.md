# mkdocs.plugin Lifecycle Hook Runtime Environment Configuration Management: Lifecycle Hooks Requiring External Service Credentials

These rules are ALWAYS ACTIVE for documentation plugins implementing lifecycle hook contracts and build extensions requiring dynamic execution flags or external service credentials.

### Rules

- **R-MKDOCS-001** MUST: Lifecycle hooks requiring external service credentials for execution MUST access them through environment mappings and raise PluginError when mandatory configuration prerequisites are absent.

### Verify

```bash
# Discover and run the project test suite to verify documentation plugin lifecycle hooks handle both present and absent environment variables
# (Command discovered from repository structure, e.g., pytest)
pytest

# Execute the documentation build verification procedure under clean environment settings
# (Command discovered from repository structure, e.g., mkdocs build)
mkdocs build
```

**Accept when:**
- Documentation build lifecycle hooks execute successfully when external credentials and environment flags are omitted in local environments.
- Execution paths requiring external integration tokens abort with descriptive plugin errors when mandatory environment variables are missing during deployment builds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing hardcoded credentials or unvalidated environment variable lookups will be rejected during automated verification and review.
</enforcement>