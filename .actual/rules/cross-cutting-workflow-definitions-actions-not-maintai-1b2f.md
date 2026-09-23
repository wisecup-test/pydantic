# Consolidation of pydantic-core CI Workflows and Introduction of Zizmor Linting: Workflow Definitions Actions Not Maintained Within

These rules are ALWAYS ACTIVE for all workflow definitions and action configurations within the repository.

### Rules

- **R-CI-001** MUST_NOT: Workflow definitions and actions MUST NOT be maintained within subpackage directories such as pydantic-core/.github/.

### Verify

```bash
# Execute zizmor security linter on workflows
zizmor .github/workflows/

# Verify no workflow or action files exist in subpackage directories
find . -name ".github" -type d | grep -v "^\./\.github$"
```

**Accept when:**
- Workflow security linter runs without errors or security warnings across all workflow files.
- No workflows or custom actions exist within pydantic-core/.github/.
- All build, test, and release jobs run successfully from root .github/workflows.

<enforcement>
Claude Code MUST NOT skip or defer verification. All workflow definitions and custom actions must be maintained strictly within root .github/ directories.
</enforcement>