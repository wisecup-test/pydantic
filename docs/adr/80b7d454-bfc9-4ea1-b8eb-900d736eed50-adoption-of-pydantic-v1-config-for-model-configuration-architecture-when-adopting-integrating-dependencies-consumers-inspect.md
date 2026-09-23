# Adoption of pydantic.v1.config for Model Configuration Architecture: When Adopting Integrating Dependencies Consumers Inspect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Specialized data validation components such as environment settings loaders and function argument decorators require declarative controls over attribute parsing and field allowance policies.
- Managing configuration parameters directly on model instances risks attribute namespace collisions with validated domain fields.
- The internal module pydantic.v1.config provides BaseConfig and associated contracts to govern validation behavior uniformly across modeling components.

## Problem Statement

Specialized validation abstractions, environment loaders, and function argument decorators require declarative controls over attribute parsing, field tolerance, and multi-source value resolution without polluting model instance namespaces or relying on mutable global registries.

## Decision

1. MUST: When adopting or integrating dependencies, consumers MUST inspect the repository lock artifact to verify the exact resolved dependency version prior to configuring model behaviors.

## Policy Block

- MUST When adopting or integrating dependencies, consumers MUST inspect the repository lock artifact to verify the exact resolved dependency version prior to configuring model behaviors.

In scope:
- Validation models, settings loaders, and argument decorators implemented within the core library namespace.
- Components requiring custom source resolution or strict field validation policies.

Out of scope:
- Unvalidated internal helper data structures and primitive type definitions.
- External client payload schemas that do not extend the framework model hierarchy.

## Rationale

- Declarative inner configuration classes isolate validation and parsing behavior from runtime data fields, preventing namespace collisions between configuration parameters and domain data.
- Standardizing configuration hooks through BaseConfig contracts enables composable source priority customization and centralized metadata preparation across specialized models.
- Explicit field tolerance controls via Extra enforcements ensure robust input validation and defensive argument handling across functional decorators and settings loaders.

## Consequences

Positive:
- Establishes a uniform declarative interface for tuning model parsing and validation behavior across diverse modules.
- Encapsulates configuration logic within model class definitions, eliminating global state and external configuration registries.
- Permits modular overriding of source hierarchies and environment field mappings via structured classmethod hooks.

Negative:
- Inner configuration classes introduce structural inheritance overhead and metaclass processing complexity during model initialization.
- Debugging configuration hook evaluation order requires navigating class hierarchy resolution across base configuration layers.

## Alternatives

- Passing runtime configuration dictionaries directly into model initializers (rejected)
  Rejected because: Runtime configuration parameter dictionaries pollute instance argument namespaces, degrade type safety, and hinder static validation of configuration schemas.
  When valid: Dynamic ad-hoc validation scenarios where model schemas are constructed ephemerally without class definitions.
- Global configuration module with central settings registries (rejected)
  Rejected because: Global configuration registries create tight coupling across distinct components, prevent isolated model customizations, and introduce mutable shared state.
  When valid: Monolithic applications with uniform, unvarying validation constraints across all domain models.

## Risks

- Inconsistent configuration inheritance when sub-classing custom models with divergent configuration inner classes.
  Mitigation: Derive all specialized model configurations directly from BaseConfig or an explicit custom configuration base class that preserves required field settings.
  Owner: Core engineering team
- Performance latency during class generation from dynamic configuration field preparation hooks.
  Mitigation: Cache prepared field metadata on model field attributes during model class creation rather than computing mappings per instance.
  Owner: Core engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure custom model implementations subclass BaseConfig when declaring inner configuration classes to guarantee standard defaults and hook compatibility.
- Utilize classmethod hooks within configuration classes to modify source priorities or parse environment variables rather than mutating global module state.

## Continuation Context


Verify commands:
- Discover and run the repository test suite to verify model configuration behavior and validation source ordering.
- Execute the repository static type verification workflow to ensure configuration inner classes satisfy base configuration type constraints.

Accept when:
- All model and settings test suites pass with expected configuration behaviors and source resolution precedence verified.
- Static type analysis confirms inner configuration classes conform to BaseConfig specifications with zero type errors.

## Enforcement

- Verified by: Automated continuous integration test pipelines verifying model validation and source resolution contracts.
- Verified by: Architecture and code review inspections ensuring model implementations define inner configuration classes adhering to BaseConfig.
- Violation handling: Pull requests introducing model implementations without standard inner configuration patterns must be revised prior to merge approval.
- Violation handling: Violations detected by test suites or static analysis will trigger build failures blocking deployment.
- Exception process: Exceptions require formal architectural review and approval from the core engineering team documented in project tracking records.