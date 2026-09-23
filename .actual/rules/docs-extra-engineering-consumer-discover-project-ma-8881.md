# MathJax Reactive Typesetting Integration with Document Observable Stream: Engineering Consumer Discover Project Manifest Package

These rules are ALWAYS ACTIVE for client-side documentation asset scripts integrating mathematical expression rendering engines and dynamic document transition/page update lifecycle subscriptions.

### Rules

- **R-MATH-001** MUST: The engineering consumer MUST discover the project manifest and package resolution lock artifact to verify the exact resolved version of the MathJax runtime before authoring or modifying typesetting lifecycle hooks.

### Verify

```bash
# Discover the documentation build and client-asset validation scripts from the repository configuration manifest and execute them.
# Run the project linting and static analysis suite to verify syntax and API call adherence within client-side asset scripts.
```

**Accept when:**
- The project client-side asset validation suite executes without errors or warnings.
- Documentation navigation transitions execute the full MathJax state reset and typesetting sequence without console errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, code review checks, and end-to-end browser testing are mandatory for client documentation integration scripts.
</enforcement>