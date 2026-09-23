# mkdocs.plugin Lifecycle Hook Runtime Environment Configuration Management: Plugin Implementations Not Hardcode External Service

These rules are ALWAYS ACTIVE for documentation plugins implementing lifecycle hook contracts and build extensions requiring dynamic execution flags or external service credentials.

### Rules

- **R-MKDOCS-001** MUST_NOT: Plugin implementations MUST NOT hardcode external service credentials or commit environment-specific configuration values to static document tree definitions.

### Verify

```bash
# Discover and run the project test suite to verify that documentation plugin lifecycle hooks handle both present and absent environment variables as specified.
# Discover and execute the documentation build verification procedure under clean environment settings to ensure local preview generation succeeds without external credentials.
```

**Accept when:**
- Documentation build lifecycle hooks execute successfully when external credentials and environment flags are omitted in local environments.
- Execution paths requiring external integration tokens abort with descriptive plugin errors when mandatory environment variables are missing during deployment builds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites executing plugin lifecycle test cases with mocked environment variables and peer code review enforce compliance; violations result in pull request rejection.
</enforcement>