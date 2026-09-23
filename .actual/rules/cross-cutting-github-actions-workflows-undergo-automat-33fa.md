# Consolidation of pydantic-core CI Workflows and Introduction of Zizmor Linting: Github Actions Workflows Undergo Automated Security

These rules are ALWAYS ACTIVE for all GitHub Actions workflows, custom reusable actions, and repository CI configurations.

### Rules

- **R-CI-001** MUST: All GitHub Actions workflows MUST undergo automated security linting enforced via zizmor.
- **R-CI-002** MUST: All GitHub Actions workflows and custom reusable actions MUST reside in the root repository directories (`.github/workflows/` and `.github/actions/`), with no workflow or action definitions permitted in subpackage directories.

### Verify

```bash
# Execute zizmor security linter to validate workflow configurations
zizmor .github/workflows/

# Verify no workflow or action files exist in subpackage directories
find . -name ".github" -type d
```

**Accept when:**
- Workflow security linter runs without errors or security warnings across all workflow files.
- No workflows or custom actions exist within subpackage directories like `pydantic-core/.github/`.
- All build, test, and release jobs run successfully from root `.github/workflows`.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>