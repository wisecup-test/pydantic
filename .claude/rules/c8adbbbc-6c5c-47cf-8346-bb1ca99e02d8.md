<rule_activation id="c8adbbbc-6c5c-47cf-8346-bb1ca99e02d8" title="Establish Public API Contracts for Design System Components: Public Contracts Versioned" applies_to="**/*">
These rules are ALWAYS ACTIVE for all design system components, theme configuration interfaces, component prop interfaces, design tokens, and public utility functions.
</rule_activation>

### Rules

- **R-API-001** MUST: Public API contracts MUST be versioned and follow semantic versioning principles where breaking changes increment major versions.

### Verify

```bash
# Count components with explicit public API markers
grep -r "@public" src/components/ | wc -l

# Run contract validation test suite
npm run validate-contracts

# Detect API breaking changes in commits
git diff HEAD~1 --name-only | xargs npm run check-breaking-changes
```

**Accept when:**
- All public-facing components have documented API contracts with validation schemas
- Contract validation tests pass in CI/CD pipeline with 100% coverage of public APIs
- Breaking change detection tooling is integrated and flags any modifications to public contracts without version bumps
- Component documentation auto-generates from contract definitions showing inputs, outputs, and constraints

<enforcement>
Claude Code MUST NOT skip or defer verification. Contract validation is mandatory in CI/CD pipeline and blocks merges on violations. All new public components require contract definitions reviewed by design system team.
</enforcement>