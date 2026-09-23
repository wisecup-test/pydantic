# Consolidation of pydantic-core CI Workflows and Introduction of Zizmor Linting: Build Test Release Workflow Definitions Maintained

These rules are ALWAYS ACTIVE for all repository GitHub Actions workflows, reusable actions, and CI/CD pipeline definitions.

### Rules

- **R-CI-001** MUST: All build, test, and release workflow definitions MUST be maintained in the root .github/workflows directory.
- **R-CI-002** MUST: Maintain custom reusable actions under root .github/actions to allow shared access and unified maintenance.
- **R-CI-003** MUST: Enforce zizmor static security analysis across all CI definitions.

### Verify

```bash
# Run zizmor workflow security linter to validate all action and workflow configurations
if command -v zizmor &> /dev/null; then
    zizmor .github/
else
    echo "zizmor not installed; skipping static security lint"
fi

# Verify no workflow or action files exist in subpackage directories (e.g., pydantic-core/.github/)
if [ -d "pydantic-core/.github" ]; then
    find pydantic-core/.github -name "*.yml" -o -name "*.yaml" | grep .
    if [ $? -eq 0 ]; then
        echo "ERROR: Workflow or action files found in subpackage directory."
        exit 1
    fi
fi
```

**Accept when:**
- Workflow security linter runs without errors or security warnings across all workflow files.
- No workflows or custom actions exist within pydantic-core/.github/.
- All build, test, and release jobs run successfully from root .github/workflows.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>