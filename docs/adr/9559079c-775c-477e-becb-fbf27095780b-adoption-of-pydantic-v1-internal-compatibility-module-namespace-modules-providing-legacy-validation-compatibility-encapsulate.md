# Adoption of pydantic.v1 Internal Compatibility Module Namespace: Modules Providing Legacy Validation Compatibility Encapsulate

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Architectural evolution of core validation and parsing capabilities introduces breaking paradigm shifts in model configurations and type handling.
- Downstream integrations and existing submodules require sustained access to prior-generation validation behaviors, decorators, and network primitives.
- Preserving previous architectural contracts directly within primary public namespaces introduces contract ambiguity and technical debt.
- An isolated compatibility namespace provides structural encapsulation for historical interfaces while the primary framework evolves.

## Problem Statement

Evolving validation engines and schema architectures require deprecating older configuration models and validation decorators without immediately breaking callers that depend on earlier public contracts and network parsing primitives.

## Decision

1. MUST: Modules providing legacy validation compatibility MUST encapsulate model validation schemas, argument decoration logic, and network parsing primitives strictly within the pydantic.v1 internal module namespace.

## Policy Block

- MUST Modules providing legacy validation compatibility MUST encapsulate model validation schemas, argument decoration logic, and network parsing primitives strictly within the pydantic.v1 internal module namespace.

In scope:
- Internal modules implementing legacy validation models, argument decorators, network parsing primitives, and datetime parsing routines.
- Compatibility layers maintaining backward-facing interfaces for consumers transitioning across framework iterations.

Out of scope:
- Modern core validation pipelines and serialization engines implemented outside the compatibility module namespace.
- External consumer applications defining native domain models without backward-compatibility requirements.

Exceptions:
- EXC-20-001: A downstream module requires direct access to low-level parsing routines without model overhead during performance-critical ingest paths

## Rationale

- Encapsulating legacy parsing and validation within the pydantic.v1 module namespace isolates deprecated execution paths and prevents schema pollution across core runtime boundaries.
- Retaining distinct types like AnyUrl, Parts, and DecoratorBaseModel within an isolated namespace maintains contract stability for existing integrations without hindering evolutionary architecture.
- Centralizing legacy configuration classes like BaseConfig and CustomConfig within the namespace ensures predictable configuration inheritance across legacy models.

## Consequences

Positive:
- Establishes a clean architectural boundary separating legacy validation contracts from active core framework internals.
- Preserves backward compatibility for callers dependent on argument validation, specialized network types, and datetime parsing routines.
- Prevents unintended coupling between legacy validator mechanics and newer runtime engines.

Negative:
- Increases repository maintenance overhead by requiring ongoing upkeep of parallel module namespace hierarchies.
- Requires strict enforcement of import boundaries to prevent developers from conflating legacy and current validation paradigms.

## Alternatives

- Complete immediate deprecation and removal of legacy validation interfaces, network types, and decorators without a transitional namespace (rejected)
  Rejected because: Breaks downstream consumer integrations and prevents progressive migration across major architectural iterations
  When valid: Acceptable only in greenfield systems with zero existing callers or backward compatibility commitments
- In-place co-location of legacy validators and models alongside modernized runtime definitions within root package namespaces (rejected)
  Rejected because: Pollutes root public namespace contracts and entangles conflicting configuration paradigms
  When valid: Acceptable in small libraries where structural divergence between versions is minimal

## Risks

- Accidental import of compatibility classes into modern subsystem implementations, re-introducing legacy paradigms.
  Mitigation: Enforce import perimeter checks in automated static analysis and continuous integration lint suites.
  Owner: Architecture and Maintainer Team
- Drift between legacy network validation rules and updated standard specifications.
  Mitigation: Maintain dedicated regression suites verifying AnyUrl and parsing contracts against standardized test vectors.
  Owner: Core Library Maintainers

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all legacy argument inspection logic delegates to validate_arguments and DecoratorBaseModel rather than reimplementing signature validation manually.
- Verify that network and uniform resource identifier parsing utilizes structured Parts and HostParts data representations for field normalization.

## Continuation Context


Verify commands:
- Discover the project test execution runner from repository build manifests and run the compatibility test suite targeting the legacy module namespace.
- Discover the project static analysis and linting configuration from the repository and execute import boundary validation to detect unauthorized imports from the legacy module namespace.

Accept when:
- All automated test suites exercising legacy validation decorators, network data structures, and datetime parsing routines pass without error.
- Static analysis verifies zero unintended cross-boundary import couplings from the compatibility module namespace into modern core namespaces.

## Enforcement

- Verified by: Continuous integration test matrix executing compatibility test suites.
- Verified by: Static analysis linters enforcing import boundaries and architectural layering constraints.
- Verified by: Peer code review verification during pull request review workflows.
- Violation handling: Automated continuous integration checks fail upon detection of disallowed imports or broken compatibility contracts.
- Violation handling: Pull requests introducing regressions in legacy namespace behavior require remediation prior to review approval.
- Exception process: Formal exception request submitted to core architecture maintainers documenting necessity and planned migration timeline.
- Exception process: Temporary architectural waiver recorded in module documentation with an agreed review milestone.