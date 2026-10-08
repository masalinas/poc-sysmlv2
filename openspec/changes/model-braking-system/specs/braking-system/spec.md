# Spec Delta

## Purpose

Defines the structural architecture, physical attribute types, subsystem compositions, and safety requirements for a vehicle braking system in SysML v2 textual notation.

## ADDED Requirements

### Requirement: Braking System Package Structure
The model SHALL define a root package named `BrakingSystem` that imports standard scalar types with private visibility (`private import ScalarValues::*;`).

#### Scenario: Package definition and imports
- **WHEN** the model file is parsed by a SysML v2 parser
- **THEN** package `BrakingSystem` is recognized and `ScalarValues::*` is imported with private visibility

### Requirement: Physical Attribute Definitions
The model SHALL define physical attribute definitions `Mass` and `Pressure` that specialize the standard scalar `Real` type.

#### Scenario: Attribute definition specialization
- **WHEN** attribute definitions `Mass` and `Pressure` are analyzed
- **THEN** both attributes specialize `Real` in compliance with KerML metamodel rules

### Requirement: Subsystem Component Definitions and Composition
The model SHALL define structural part definitions `BrakePedal`, `BrakeCaliper`, and `Vehicle`. The `Vehicle` part definition SHALL compose a `mass` attribute of type `Mass`, exactly one `brakePedal` part of type `BrakePedal` (`[1]`), and four `brakeCalipers` parts of type `BrakeCaliper` (`[4]`).

#### Scenario: Vehicle part composition
- **WHEN** the `Vehicle` part definition is evaluated
- **THEN** it contains a `mass` attribute of type `Mass`, one `brakePedal` of multiplicity 1, and four `brakeCalipers` of multiplicity 4

### Requirement: Stopping Distance Requirement Definition
The model SHALL define a requirement definition `MaxStoppingDistance` containing a doc comment explaining the stopping threshold constraints.

#### Scenario: Requirement doc comment inspection
- **WHEN** the `MaxStoppingDistance` requirement is inspected
- **THEN** it contains a documentation comment specifying emergency stopping criteria

### Requirement: SysML v2 Metamodel Validation
The model file `braking.sysml` SHALL pass validation using the official OMG SysML v2 Pilot Implementation tool (`validate-sysml`) with zero errors.

#### Scenario: Successful validator execution
- **WHEN** `validate-sysml braking.sysml` is executed
- **THEN** the validator completes with exit code 0 and reports zero syntax or semantic diagnostics
