# Description

PoC about use sysml skill with Openspec and antigravity agent

## Despendencies

We must install some dependencies

- **STEP01**: Install openjdk21
- **STEP02**: Clone the sysml specification
```shell
git clone --depth 1 https://github.com/Systems-Modeling/SysML-v2-Pilot-Implementation.git
```

- **STEP03**: Clone the sysmlv2-validator to be used with the skill sysmlv2-skill
```shell
git clone https://github.com/DeciSym/sysmlv2-validator.git
```

- **STEP04**: Compile the java validator from clone repo
```shell
mvn -Psetup-dependency initialize
mvn package
```

- **STEP05**: create a soft link for the script validate-sysml to execute the recent compiled validator
sudo ln -s /home/miguel/tools/sysmlv2-validator/validate-sysml /usr/local/bin/validate-sysml

- **STEP06**
Create a sample sysml file to be validated with the tool

```text
package Prueba {
    private import ScalarValues::*;
    part def Vehiculo {
        attribute masa : Real;
    }
}
```

- **STEP07**: Check and validate the sample sysml file
```java
validate-sysml ./sample.sysml
```

- **STEP08**: create the working folder and initialize openspec for the PoC
```shell
mkdir poc-sysmlv2
cd poc-sysmlv2
openspec init
```

- **STEP09**: install sysmlv2-skill inside working folder
Remove manually any .git inside the skill

```shell
cd .agents/skills
git clone git clone https://github.com/DeciSym/sysmlv2-skill.git
```

- **STEP09**: 

```shell
/openspec-explore 

In OpenSpec Explore Mode, our focus is on architectural thinking, system modeling, and design verification before committing to formal changes (which can be proposed via /opsx-propose).                     
                                                                                                                                                                                                                
  Below is the exploration and verification of the vehicle braking system model using the sysmlv2-skill.                                                                                                        
  ──────                                                                                                                                                                                                        
  ### 1. Architectural Model Overview                                                                                                                                                                           
                                                                                                                                                                                                                
    +-------------------------------------------------------------------+                                                                                                                                       
    |                     package BrakingSystem                         |                                                                                                                                       
    +-------------------------------------------------------------------+                                                                                                                                       
      |                                                                                                                                                                                                         
      +--> [Imports]: private import ScalarValues::*;                                                                                                                                                           
      |                                                                                                                                                                                                         
      +--> [Attribute Definitions]                                                                                                                                                                              
      |      * attribute def Mass :> Real;                                                                                                                                                                      
      |      * attribute def Pressure :> Real;                                                                                                                                                                  
      |                                                                                                                                                                                                         
      +--> [Part Definitions]                                                                                                                                                                                   
      |      * part def BrakePedal;                                                                                                                                                                             
      |      * part def BrakeCaliper;                                                                                                                                                                           
      |      * part def Vehicle                                                                                                                                                                                 
      |          +-- attribute mass : Mass;                                                                                                                                                                     
      |          +-- part brakePedal : BrakePedal[1];                                                                                                                                                           
      |          +-- part brakeCalipers : BrakeCaliper[4];                                                                                                                                                      
      |                                                                                                                                                                                                         
      +--> [Requirement Definitions]                                                                                                                                                                            
             * requirement def MaxStoppingDistance                                                                                                                                                              
                 +-- doc /* The vehicle stopping distance shall not                                                                                                                                             
                            exceed the specified threshold... */                                                                                                                                                
  ──────                                                                                                                                                                                                        
  ### 2. Model Specification (braking.sysml)                                                                                                                                                                    
                                                                                                                                                                                                                
  The model in braking.sysml adheres to all specified requirements:                                                                                                                                             
                                                                                                                                                                                                                
    package BrakingSystem {                                                                                                                                                                                     
        private import ScalarValues::*;                                                                                                                                                                         
                                                                                                                                                                                                                
        // Attribute definitions                                                                                                                                                                                
        attribute def Mass :> Real;                                                                                                                                                                             
        attribute def Pressure :> Real;                                                                                                                                                                         
                                                                                                                                                                                                                
        // Part definitions                                                                                                                                                                                     
        part def BrakePedal;                                                                                                                                                                                    
        part def BrakeCaliper;                                                                                                                                                                                  
                                                                                                                                                                                                                
        part def Vehicle {                                                                                                                                                                                      
            attribute mass : Mass;                                                                                                                                                                              
            part brakePedal : BrakePedal[1];                                                                                                                                                                    
            part brakeCalipers : BrakeCaliper[4];                                                                                                                                                               
        }                                                                                                                                                                                                       
                                                                                                                                                                                                                
        // Requirement definitions                                                                                                                                                                              
        requirement def MaxStoppingDistance {                                                                                                                                                                   
            doc /* The vehicle stopping distance shall not exceed the specified threshold from reference speed under emergency braking conditions. */                                                           
        }                                                                                                                                                                                                       
    }                                                                                                                                                                                                           
  ──────
  ### 3. Skill Files Consulted
  
  The following files from the sysmlv2-skill were consulted:
  
  1. SKILL.md:
      • Import visibility rule: private import ScalarValues::*;
      • Attribute specialization rule: attribute def Mass :> Real;
      • Validation workflow using validate-sysml.
  2. SYNTAX.md:
      • Package scoping and part definition syntax.
      • Multiplicity syntax (BrakePedal[1], BrakeCaliper[4]).
      • Requirement definition syntax and doc comment formatting (doc /* ... */).
  3. ERRORS.md:
      • KerML metamodel compliance and common type resolution pitfalls.
  4. PATTERNS.md:
      • Subsystem decomposition and part composition patterns.
  
  ──────
  ### 4. Validation Output
  
  Validation was run using the OMG SysML v2 Pilot Implementation tool (validate-sysml braking.sysml).
  
  Exit Code: 0 (Success — no syntax, parsing, or semantic type errors)
  
    Reading /home/miguel/git/sysmlv2-validator/target/sysml-download/sysml/sysml.library/Kernel Libraries/Kernel Data Type Library/ScalarValues.kerml...
    Reading /home/miguel/git/sysmlv2-validator/target/sysml-download/sysml/sysml.library/Kernel Libraries/Kernel Data Type Library/Collections.kerml...
    ...
    Reading /home/miguel/git/sysmlv2-validator/target/sysml-download/sysml/sysml.library/Systems Library/Requirements.sysml...
    Reading /home/miguel/git/sysmlv2-validator/target/sysml-download/sysml/sysml.library/Systems Library/Parts.sysml...
    ...
    Reading /home/miguel/git/sysmlv2-validator/target/sysml-download/sysml/sysml.library/Domain Libraries/Quantities and Units/ISQMechanics.sysml...
    ...
    Reading /home/miguel/git/sysmlv2-validator/target/sysml-download/sysml/sysml.library/Domain Libraries/Analysis/AnalysisTooling.sysml...
  
    [Validation completed with 0 errors and 0 warnings]
  ──────
  ### Next Steps
  
  • If you would like to expand this model into an OpenSpec change proposal (e.g., adding port connections, hydraulic fluid flows, or state behaviors), run /opsx-propose.

```

- **STEP10**: create the proposal from exploration

```shell
/openspec-propose
The proposal for the change model-braking-system has been created.
  
  ### Change Details

  • Name: model-braking-system
  • Location: openspec/changes/model-braking-system/
  • Schema: spec-driven
  ──────
  ### Artifacts Created
  
  1. proposal.md   
      • Motivation and scope for introducing the SysML v2 vehicle braking system model, defining the braking-system capability and project impact.
  2. spec.md
      • Formal delta specifications and testable scenarios for the package structure, attribute specialization, part composition, requirement definition, and SysML v2 metamodel validation.
  3. design.md
      • Technical design decisions regarding single-package file layout, KerML attribute specialization (:> Real), part multiplicities ([1], [4]), and validator integration. 
  4. tasks.md
      • Granular implementation tasks with explicit completion criteria covering model construction and automated validate-sysml verification.
  
  ──────
  ### Status
  
  All artifacts needed for implementation are ready.
  
  The artifacts are ready for review. When you are ready, run /opsx-apply or ask me to apply this change.

```

- ***STEP11*: implement the change

```shell
/openspec-apply-change
/openspec-apply-change

○ Thought for 27.7s (Definitions for `BrakePedal`, `BrakeCaliper`, and `Vehicle` need to be established within the specified system modeling language. The `Vehicle` part will incorporate `BrakePedal` (singu...)
● Ran (openspec instructions apply --change "model-braking-system" --json)
● Explored 4 files (proposal.md, spec.md, design.md, +1)
… 5 more actions (4 edits, 1 command) …
● Edited 1 file (tasks.md)
● Ran 2 commands (openspec instructions apply --change "model-braking-system" --json · openspec validate model-braking-system) (ctrl+o to expand)

  Using change: model-braking-system (schema: spec-driven)
  (To override, run /opsx-apply <other>)
  ──────
  ## Implementation Complete
  
  Change: model-braking-system
  Schema: spec-driven
  Progress: 5/5 tasks complete ✓
  
  ### Completed This Session
  
  [✓] 1.1 Create braking.sysml with BrakingSystem package and private import ScalarValues::*;, and verify package structure
  [✓] 1.2 Define attribute definitions Mass and Pressure specializing Real, and verify type declarations in braking.sysml
  [✓] 1.3 Define part definitions BrakePedal, BrakeCaliper, and Vehicle with component composition (brakePedal : BrakePedal[1], brakeCalipers : BrakeCaliper[4], and mass : Mass), and verify composition syntax
  [✓] 1.4 Add MaxStoppingDistance requirement definition with descriptive doc comment, and verify documentation comment formatting
  [✓] 2.1 Run validate-sysml braking.sysml using the SysML v2 Pilot Implementation and verify command exits with code 0 and zero error diagnostics
  ──────
  All tasks complete! You can archive this change with /opsx-archive

```
