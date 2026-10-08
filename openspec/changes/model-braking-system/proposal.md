# Proposal

## Why

Establish a formal SysML v2 architectural model for a vehicle braking system within the repository. Defining the physical and structural aspects in standardized SysML v2 textual notation allows rigorous validation against OMG KerML metamodels, ensuring consistent subsystem decomposition and requirement traceability for automotive systems engineering.

## What Changes

- Model a vehicle braking system package (`BrakingSystem`) in SysML v2 textual notation.
- Define physical attribute types (`Mass`, `Pressure`) specializing standard scalar types.
- Define structural component definitions (`BrakePedal`, `BrakeCaliper`, and `Vehicle`).
- Establish composition relationships where `Vehicle` contains one `BrakePedal` and four `BrakeCaliper` parts alongside a `mass` attribute.
- Define a system-level requirement (`MaxStoppingDistance`) with documentation comments.
- Integrate automated validation via the OMG Pilot Implementation (`validate-sysml`).

## Capabilities

### New Capabilities
- `braking-system`: Defines and validates the vehicle braking system structural architecture, attribute types, part compositions, and requirements in SysML v2.

### Modified Capabilities
<!-- None -->

## Impact

- Adds `braking.sysml` model file in the repository workspace.
- Requires SysML v2 validation tooling (`validate-sysml` / OMG SysML v2 Pilot Implementation).
- Introduces baseline specifications for vehicle braking architecture in OpenSpec.
