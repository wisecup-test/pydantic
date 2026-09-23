# Consolidation of pydantic-core CI Workflows and Introduction of Zizmor Linting: Reusable Build Packaging Actions Submodules Located

These rules are ALWAYS ACTIVE for all GitHub Actions workflows, reusable build and packaging actions, and subpackage workflow configurations.

### Rules

- **R-CI-001** MUST: Reusable build and packaging actions for submodules MUST be located within the root .github/actions/ directory hierarchy.

### Verify

```bash
# Execute the project workflow security linter (zizmor) to validate all action and workflow configurations
zizmor --config .github/zizmor.yml .github/workflows/ .github/actions/

# Execute repository structure checks to verify no workflow or action files exist in subpackage directories
! find . -path "*/.github/workflows/*" -o -path "*/.github/actions/*" | grep -v "^./\.github"
```

**Accept when:**
- Workflow security linter runs without errors or security warnings across all workflow files.
- No workflows or custom actions exist within pydantic-core/.github/.
- All build, test, and release jobs run successfully from root .github/workflows.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification via automated security linting and repository structure checks is mandatory for all changes related to CI workflows and actions.
</enforcement>