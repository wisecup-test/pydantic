# MathJax Reactive Typesetting Integration with Document Observable Stream: Mathematical Typesetting Integration Routines Not Invoke

These rules are ALWAYS ACTIVE for client-side documentation asset scripts integrating mathematical expression rendering engines and dynamic document transition and page update lifecycle subscriptions.

### Rules

- **R-MATH-001** MUST_NOT: Mathematical typesetting integration routines MUST NOT invoke MathJax.typesetPromise without first resetting TeX state via MathJax.texReset and clearing previous rendered structures via MathJax.typesetClear.

### Verify

```bash
# Discover the documentation build and client-asset validation scripts from the repository configuration manifest and execute them.
# Run the project linting and static analysis suite to verify syntax and API call adherence within client-side asset scripts.
```

**Accept when:**
- The project client-side asset validation suite executes without errors or warnings.
- Documentation navigation transitions execute the full MathJax state reset and typesetting sequence without console errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and code review checks against client documentation integration scripts enforce these rules.
</enforcement>