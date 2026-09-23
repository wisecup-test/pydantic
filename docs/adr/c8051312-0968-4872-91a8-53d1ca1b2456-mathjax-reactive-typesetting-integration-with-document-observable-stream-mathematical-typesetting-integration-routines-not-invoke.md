# MathJax Reactive Typesetting Integration with Document Observable Stream: Mathematical Typesetting Integration Routines Not Invoke

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Client-side documentation pages utilize dynamic navigation transitions where document contents update reactively without a full browser reload.
- Static mathematical expressions require dynamic re-typesetting by the MathJax rendering engine whenever the active document body changes.
- Without explicit cache invalidation and state reset routines, repeated client-side navigations retain stale typesetting caches and corrupted equation numbering counters.

## Problem Statement

When documentation sites update DOM content dynamically through reactive document streams, third-party rendering engines such as MathJax do not automatically detect DOM replacements, resulting in unrendered mathematical equations, stale cached rendering artifacts, and corrupted equation numbering state across page transitions.

## Decision

1. MUST_NOT: Mathematical typesetting integration routines MUST NOT invoke MathJax.typesetPromise without first resetting TeX state via MathJax.texReset and clearing previous rendered structures via MathJax.typesetClear.

## Policy Block

- MUST_NOT Mathematical typesetting integration routines MUST NOT invoke MathJax.typesetPromise without first resetting TeX state via MathJax.texReset and clearing previous rendered structures via MathJax.typesetClear.

In scope:
- Client-side documentation asset scripts integrating mathematical expression rendering engines.
- Dynamic document transition and page update lifecycle subscriptions.

Out of scope:
- Server-rendered static documentation where pages load via full browser refreshes without client-side navigation streams.
- Non-mathematical documentation pages that do not include mathematical typesetting markup.

## Rationale

- Binding typesetting re-evaluation directly to the document$ observable stream ensures that every dynamic document transition triggers mathematical equation rendering deterministically.
- Clearing the output cache via MathJax.startup.output.clearCache and clearing typeset state via MathJax.typesetClear guarantees that modified equations are fully re-rendered rather than populated from stale cache artifacts.
- Executing MathJax.texReset resets equation counters and macro definitions, preventing cross-page equation numbering corruption during continuous browsing sessions.

## Consequences

Positive:
- Equations render accurately and consistently across all dynamic document navigations without requiring full page reloads.
- Equation numbering counters and macro scopes remain isolated and accurate across distinct document pages.
- Rendering cache invalidation prevents visual artifacts from previously visited pages.

Negative:
- Executing cache clearance and typesetting promises on every navigation event introduces minor client-side processing latency.
- Tight coupling to the document$ observable contract creates a maintenance dependency on the host documentation navigation lifecycle.

## Alternatives

- Relying on standard full-page browser navigation without reactive document observable streams (rejected)
  Rejected because: Full page reloads degrade client navigation performance and negate the benefits of client-side instant navigation.
  When valid: Static documentation sites where client-side navigation streams are disabled or unsupported.
- Invoking MathJax.typesetPromise directly on navigation without clearing cache or resetting TeX counters (rejected)
  Rejected because: Omitting MathJax.startup.output.clearCache, MathJax.typesetClear, and MathJax.texReset produces cumulative equation numbering increments and visual rendering corruption across navigation events.
  When valid: Single-page static documents where equations are rendered exactly once and navigation transitions never occur.

## Risks

- Asynchronous typesetting via MathJax.typesetPromise may encounter unhandled promise rejections on malformed LaTeX expressions.
  Mitigation: Attach error rejection callbacks to the typesetting promise and validate mathematical expressions during documentation validation builds.
  Owner: Documentation engineering team
- Upstream changes in the MathJax API surface across major versions could alter or remove reset and cache clearing methods.
  Mitigation: Verify API signatures against the authoritative locked dependency documentation as specified in the mandatory discovery policy.
  Owner: Documentation engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure that client-side integration scripts register subscriptions after the host document observable stream has been initialized on the window or global document scope.
- Maintain the exact sequential order of cache invalidation, typeset clearing, TeX reset, and asynchronous typesetting to avoid race conditions during document transitions.

## Continuation Context


Verify commands:
- Discover the documentation build and client-asset validation scripts from the repository configuration manifest and execute them.
- Run the project linting and static analysis suite to verify syntax and API call adherence within client-side asset scripts.

Accept when:
- The project client-side asset validation suite executes without errors or warnings.
- Documentation navigation transitions execute the full MathJax state reset and typesetting sequence without console errors.

## Enforcement

- Verified by: Automated static analysis and code review checks against client documentation integration scripts.
- Verified by: End-to-end browser testing validating mathematical expression re-rendering across navigation transitions.
- Violation handling: Pull requests omitting required reset and cache invalidation calls in typesetting lifecycle subscriptions will be rejected during review.
- Exception process: Exceptions must be submitted via architectural review with evidence demonstrating that dynamic navigation transitions do not affect typesetting state.