# Standardize External Client Boundary Patterns in Internal APIs: Internal Utility Functions

Status: proposed
Date: 2025-01-10
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all internal API implementations that expose interfaces to external clients or require boundary validation.

## Context

- Internal APIs in the codebase require clear boundaries between internal implementation details and external client interfaces, particularly in validation, configuration, and data transformation layers
- The pattern was detected across 6 files with 91.28% confidence, primarily in pydantic's internal modules (_core_utils.py, _dataclasses.py, _core_metadata.py, _config.py) and test infrastructure, indicating a systematic approach to API boundary management
- External clients need stable, well-defined interfaces while internal implementations require flexibility to evolve without breaking client code
- The boundaries.external_clients facet suggests explicit handling of client-facing interfaces separate from internal utilities, enabling better encapsulation and API stability

## Problem Statement

Internal APIs must balance the need for internal flexibility and refactoring with the requirement to provide stable, predictable interfaces to external clients. Without clear boundary patterns, internal changes can inadvertently break client code, and the distinction between public and private APIs becomes unclear, leading to maintenance challenges and API contract violations.

## Decision

1. MUST: Internal utility functions and classes that are not intended for external use MUST be prefixed with underscore or placed in _internal modules

## Policy Block

- MUST Internal utility functions and classes that are not intended for external use MUST be prefixed with underscore or placed in _internal modules

In scope:
- All internal API modules that provide interfaces to external clients or libraries
- Validation, configuration, and data transformation layers in internal APIs
- Core utility modules that bridge internal implementations with external interfaces
- Plugin systems and extension points that external code may interact with

Out of scope:
- Pure internal implementation details with no external exposure
- Test utilities and fixtures used only within the test suite
- Temporary or experimental APIs explicitly marked as unstable
- Third-party library integrations that follow their own boundary patterns

Exceptions:
- EXC-001: Advanced users need access to internal APIs for performance optimization or deep customization not possible through public interfaces
- EXC-002: Internal testing requires direct access to implementation details for comprehensive coverage

## Rationale

- The pattern appears consistently across pydantic's internal modules with 91.28% confidence, demonstrating a proven approach to managing API boundaries in a widely-used validation library
- Clear boundary separation enables internal refactoring without breaking external clients, reducing maintenance burden and improving API stability
- The boundaries.external_clients facet indicates explicit architectural intent to manage client interactions, not accidental coupling
- Evidence from 6 files across core utilities, dataclasses, metadata, and configuration modules shows this is a systematic architectural pattern, not isolated implementation details

## Consequences

Positive:
- Internal implementations can evolve and refactor freely without breaking external client code
- Clear API contracts reduce confusion about what interfaces are stable and supported
- Validation and transformation at boundaries protect internal code from invalid states
- Improved maintainability through explicit separation of concerns between internal and external interfaces

Negative:
- Additional abstraction layers may introduce slight performance overhead in boundary crossing
- Developers must maintain discipline in respecting boundary conventions and not bypassing them
- More complex module structure with explicit internal/external separation may increase initial learning curve
- Duplication of interfaces may occur when internal and external representations diverge

## Alternatives

- Single unified API surface with no internal/external distinction (rejected)
  Rejected because: Prevents internal refactoring without breaking changes and exposes implementation details that should remain flexible
  When valid: Only appropriate for very small libraries with minimal complexity where all code is effectively public API
- Complete separation with internal and external packages as separate distributions (rejected)
  Rejected because: Adds significant deployment and versioning complexity, and may be overkill for most internal API scenarios
  When valid: Valid for very large systems where internal and external components have completely independent lifecycles
- Documentation-only boundaries without enforcement mechanisms (rejected)
  Rejected because: Relies on client discipline without technical enforcement, leading to inevitable boundary violations and coupling
  When valid: May be acceptable in small teams with strong code review culture and limited external client base

## Risks

- Developers may bypass boundary patterns for convenience, gradually eroding the separation
  Mitigation: Implement automated checks in CI to detect direct imports of internal modules by external code, and enforce through code review
  Owner: Engineering team and API maintainers
- Boundary abstractions may become outdated as internal implementations evolve, creating impedance mismatch
  Mitigation: Regular architectural reviews to ensure boundary interfaces remain aligned with internal capabilities and external needs
  Owner: Architecture team
- Performance-sensitive code paths may suffer from boundary validation overhead
  Mitigation: Profile critical paths and provide optimized fast-path options for validated internal calls while maintaining boundaries for external clients
  Owner: Performance engineering team

## Implementation Notes

- Use Python's underscore prefix convention (_internal, _private) consistently to mark internal modules and functions not intended for external use
- Implement __all__ exports in public modules to explicitly control the external API surface
- Consider using type hints and protocols to define boundary contracts without exposing implementation details
- Document the boundary pattern in architecture guides and provide examples of correct usage for both internal and external developers
- Establish clear guidelines for when to create new boundary interfaces versus extending existing ones

## Continuation Context


Verify commands:
- grep -r 'from pydantic._internal' --include='*.py' --exclude-dir='pydantic' --exclude-dir='tests' . | wc -l | grep -q '^0$'
- python -c "import ast; import sys; [sys.exit(1) for f in sys.argv[1:] if any('_internal' in n.module for n in ast.walk(ast.parse(open(f).read())) if isinstance(n, ast.ImportFrom) and n.module)]" $(find . -name '*.py' -not -path '*/pydantic/*' -not -path '*/tests/*')
- find . -path '*/_internal/*.py' -exec grep -L '^def\|^class' {} \; | xargs -r grep -L '__all__' | wc -l

Accept when:
- No external client code directly imports from _internal modules or uses underscore-prefixed APIs
- All public API modules explicitly define __all__ exports or have clear documentation of public interfaces
- Boundary validation is present in all external entry points to internal API functionality
- Code review process includes verification that new internal APIs follow boundary separation patterns

## Enforcement

- Verified by: Automated CI checks scanning for imports of _internal modules from external code
- Verified by: Code review checklist items verifying boundary pattern compliance
- Verified by: Static analysis tools configured to flag direct access to internal APIs
- Verified by: Regular architectural audits of API boundary implementations
- Violation handling: CI pipeline fails if external code imports from _internal modules
- Violation handling: Code review blocks merge if boundary patterns are violated without documented exception
- Violation handling: Deprecation warnings issued for any accidental exposure of internal APIs to external clients
- Violation handling: Quarterly reviews identify and remediate boundary violations that slip through
- Exception process: Developer submits exception request with justification to architecture team
- Exception process: Architecture team reviews whether public API extension would be more appropriate
- Exception process: If approved, exception is documented with stability warnings and version compatibility notes
- Exception process: Exceptions are reviewed quarterly to determine if they should be promoted to public APIs or removed