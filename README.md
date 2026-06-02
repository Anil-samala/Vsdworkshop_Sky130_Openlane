# vsdworkshop_sky130_openlane
This repository summarizes my learning experience and practical work completed during the VSD-IAT Digital VLSI SoC Design program. The project demonstrates the complete RTL-to-GDSII ASIC implementation flow using the SkyWater 130nm Process Design Kit (PDK) and the OpenLANE framework.The work includes different stages of ASIC Design flow
# Day 1 – Introduction to Open-Source EDA Tools, OpenLANE, and Sky130 PDK

## Software-to-Hardware Abstraction Stack

Modern digital systems are built using multiple abstraction layers, where each layer simplifies the complexity of the layer beneath it. Application software never interacts directly with transistors. Instead, communication between software and hardware occurs through the **Instruction Set Architecture (ISA)**.

Understanding these layers is essential for appreciating how software eventually becomes a physical integrated circuit.

---

## 1. Application Layer

The application layer contains programs that users interact with daily, such as:

* Calculators
* Stopwatches
* Web browsers
* Media players

Characteristics:

* Developed using high-level programming languages such as C, C++, Java, and Python.
* Independent of specific hardware implementations.
* Relies on operating systems and supporting software for execution.

---

## 2. System Software Layer

System software manages the hardware resources available in a computer system and provides services required by applications.

Major responsibilities include:

* Process scheduling
* Memory management
* Device control
* Input/Output operations

Examples:

* Linux
* Windows

This layer acts as an interface between application software and the underlying hardware.

---

## 3. Compiler and Assembler

Programs written in high-level languages cannot be executed directly by a processor. They must first be translated into machine-readable instructions.

### Compiler

The compiler converts high-level source code into assembly language instructions that follow a particular ISA.

### Assembler

The assembler translates assembly instructions into binary machine code that can be executed by the processor.

Because the ISA defines a standard interface, the same source code can target different processors using different compiler toolchains.

---

## 4. Instruction Set Architecture (ISA – RISC-V)

The Instruction Set Architecture specifies the set of instructions that a processor can execute.

It defines:

* Instruction formats
* Register architecture
* Memory access mechanisms
* Arithmetic and logical operations

### Features of RISC-V

* Open-source architecture
* Modular design
* Highly scalable
* Industry and academia friendly

The ISA serves as the contract between software and hardware, ensuring compatibility regardless of the processor implementation.

---

## 5. Microarchitecture (RTL Implementation)

Microarchitecture represents the actual hardware implementation of the ISA at the Register Transfer Level (RTL).

RTL designs are typically written using:

* Verilog
* VHDL

A processor implementation generally contains:

* Instruction decoder
* Register file
* Control logic
* Arithmetic datapath

### Example

PicoRV32 is a lightweight RISC-V processor core implemented in Verilog.

Multiple microarchitectures can implement the same ISA while providing different trade-offs in terms of performance, power consumption, and area.

---

## 6. Physical Design and Fabrication

Once the RTL design is finalized, it is transformed into a manufacturable chip through several implementation stages.

These stages include:

* Logic synthesis
* Floorplanning
* Placement
* Clock Tree Synthesis (CTS)
* Routing
* Timing verification
* Physical verification

The final outcome is a GDSII layout that can be sent to a semiconductor foundry for fabrication.

---

## Key Concept

Software is designed for a specific ISA rather than a specific hardware implementation.

As long as a processor correctly implements the ISA specification, the same software can execute on different hardware implementations without modification.

---

## Complete ASIC Design Flow (RTL to GDSII)

The standard ASIC design flow consists of the following stages:

1. Design Specification
2. RTL Design
3. Functional Verification
4. Logic Synthesis
5. Floorplanning
6. Placement
7. Clock Tree Synthesis (CTS)
8. Routing
9. Sign-off Verification (DRC, LVS, STA)
10. GDSII Generation

Each stage progressively transforms the design from a high-level hardware description into a fabrication-ready physical layout.

---

## Open-Source EDA Tools

Modern ASIC design can be performed using a variety of open-source Electronic Design Automation (EDA) tools.

| Tool     | Purpose                      |
| -------- | ---------------------------- |
| Yosys    | Logic Synthesis              |
| OpenLANE | Complete RTL-to-GDSII Flow   |
| Magic    | Layout Visualization and DRC |
| OpenSTA  | Static Timing Analysis       |
| ngspice  | Circuit Simulation           |

These tools enable designers to implement and verify integrated circuits without relying on proprietary software.

---

## Sky130 Process Design Kit (PDK)

The Sky130 PDK provides all process-specific information required for integrated circuit design and fabrication.

The kit includes:

* Device models
* Technology files
* Design rules
* Standard-cell libraries
* Characterization data

Developed by **SkyWater Technology**, the Sky130 PDK is one of the first fully open-source process technologies available to the semiconductor community.

---

## Task 1: Run Synthesis for the `picorv32a` Design Using OpenLANE

The first practical exercise involves synthesizing the PicoRV32A RISC-V processor using the OpenLANE flow.

### Launch OpenLANE and Start Synthesis

![image alt](https://github.com/Anil-samala/vsdworkshop_sky130_openlane/blob/main/flo_tcl_interactive.png)

### Synthesis Results and Statistics

![image_alt](https://github.com/Anil-samala/Vsdworkshop_Sky130_Openlane/blob/main/synthesis_success.png)


The synthesis stage converts the RTL description into a gate-level netlist using cells from the selected standard-cell library.

---

## Task 2: Calculate the Flip-Flop Ratio

After synthesis, analyze the generated reports to determine the proportion of sequential elements in the design.

### Flip-Flop Statistics

![image alt](https://github.com/Anil-samala/vsdworkshop_sky130_openlane/blob/main/task_1_flop_ratio.png)

![image alt](https://github.com/Anil-samala/vsdworkshop_sky130_openlane/blob/main/task_1_flop_ratio_dff.png)

### Flip-Flop Ratio Calculation

The percentage of flip-flops present in the design can be calculated using:
```math
[
\text{Flip-Flop Ratio} =
\frac{\text{Number of DFF Cells}}
{\text{Total Number of Cells}}
\times 100
]
```
From the synthesis report:
```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
\text{DFF Percentage} = 0.108429685 * 100 = 10.84296854\ \%
```

This metric provides insight into the sequential logic content of the design.

---

## Day 1 Summary

### Key Takeaways

* Understood the complete software-to-silicon abstraction hierarchy.
* Learned the role of compilers, assemblers, and instruction set architectures.
* Studied the significance of the RISC-V ISA.
* Explored the RTL-to-GDSII ASIC implementation flow.
* Familiarized with major open-source EDA tools used in digital VLSI design.
* Performed synthesis of the PicoRV32A processor using OpenLANE.
* Analyzed synthesis reports and calculated the flip-flop ratio of the design.


# Day 2 – Floorplanning Fundamentals and Introduction to Standard Cell Libraries

## Floorplanning

Floorplanning is one of the most important stages in the physical design flow. It determines the initial organization of the chip by defining the placement region for standard cells, I/O pins, and power distribution structures.

The primary objectives of floorplanning are:

* Defining die dimensions
* Allocating core area
* Positioning I/O ports
* Establishing power distribution networks
* Preparing the design for efficient placement and routing

A well-planned floorplan significantly improves timing performance, routability, and overall chip quality.

---

## Die Area and Core Area

Two fundamental regions are defined during floorplanning:

### Die Area

The die represents the complete silicon area that will be fabricated on the wafer. It includes:

* Core logic region
* I/O cells
* Power structures
* Routing resources

### Core Area

The core is the region inside the die where standard cells are placed.

Characteristics of the core area:

* Contains synthesized logic cells
* Excludes I/O pads and boundary structures
* Directly impacts utilization and congestion

---

## Aspect Ratio and Core Utilization

### Aspect Ratio

Aspect ratio describes the geometric shape of the floorplan and is calculated as:
```math
[
\text{Aspect Ratio}=\frac{\text{Height}}{\text{Width}}
]
```
A balanced aspect ratio helps reduce routing congestion and improves placement quality.

### Core Utilization

Core utilization indicates how much of the available core area is occupied by standard cells.
```math
[
\text{Utilization}=\frac{\text{Standard Cell Area}}{\text{Core Area}}
]
```
Higher utilization improves area efficiency but may increase routing congestion, while lower utilization provides more routing flexibility.

---

## Power Planning

Power planning establishes a reliable power delivery network throughout the design.

The power network consists of:

* VDD power rails
* VSS (Ground) rails
* Power rings
* Power straps

A robust power distribution system helps minimize:

* IR drop
* Electromigration issues
* Power integrity problems

Proper power planning is essential for stable circuit operation.

---

## Characteristics of Good and Bad Floorplans

### Good Floorplan

A well-designed floorplan provides:

* Balanced utilization
* Reduced routing congestion
* Efficient power distribution
* Better timing performance
* Easier routing closure

### Poor Floorplan

An inefficient floorplan may lead to:

* Congested routing channels
* Timing violations
* Increased IR drop
* Excessive wirelength
* Lower overall design quality

---

## Introduction to Standard Cell Libraries

Standard cell libraries contain pre-characterized logic cells that are used during synthesis and physical implementation.

These libraries generally include:

### Combinational Logic Cells

Examples:

* AND gates
* OR gates
* NAND gates
* NOR gates
* Multiplexers

### Sequential Logic Cells

Examples:

* D Flip-Flops
* Latches
* Registers

Each cell is characterized for:

* Timing
* Area
* Power consumption

This information is utilized throughout synthesis, placement, routing, and timing analysis.

---

## Task 1: Execute Floorplanning in OpenLANE

Run the floorplanning stage using the OpenLANE flow.

### Floorplan Execution

![image alt](https://github.com/Anil-samala/vsdworkshop_sky130_openlane/blob/main/floorplan.png)

---

## Task 2: Examine the Generated Floorplan DEF File

After floorplanning, open the generated DEF file and review the design dimensions.

### Opening the Floorplan DEF

*(Insert Screenshot Here)*

### Design Measurements

Given:

* 1 unit distance = 1 micron
* Die Width = 660.685 µm
* Die Height = 671.405 µm

Therefore,
```math
[
\text{Die Area}=660.685 \times 671.405
]
```
```math
[
\text{Die Area}=443587.2\ \mu m^2
]
```
---

## Task 3: Visualize the Floorplan Using Magic

Load the generated floorplan DEF file into Magic to inspect the physical layout.

### Floorplan Loaded in Magic

*(Insert Screenshot Here)*

### I/O Port Placement

The floorplan shows a uniform distribution of ports along the chip boundary. Proper port spacing improves signal accessibility and routing efficiency.

*(Insert Screenshot Here)*

---

## Task 4: Perform Congestion-Aware Placement

After floorplanning, run the placement stage using OpenLANE.

Placement determines the exact locations of standard cells inside the core area while considering timing and routing constraints.

### Running Placement

*(Insert Screenshot Here)*

---

## Task 5: Analyze Placement Results in Magic

Load the placement DEF file into Magic and inspect the placement quality.

### Placement Layout Visualization

*(Insert Screenshot Here)*

The placement stage arranges synthesized standard cells within the core while maintaining legal spacing and optimizing for timing and routability.

---

## Day 2 Summary

### Key Takeaways

* Learned the objectives and significance of floorplanning.
* Understood the difference between die area and core area.
* Studied aspect ratio and utilization calculations.
* Explored power planning concepts and power distribution structures.
* Compared characteristics of efficient and inefficient floorplans.
* Examined standard cell libraries and their role in physical design.
* Executed floorplanning and placement using OpenLANE.
* Visualized floorplan and placement results using Magic.

# Day 3 – Custom Standard Cell Design Using Magic Layout and ngspice Characterization

## Introduction to CMOS Inverter Design

The CMOS inverter is one of the most fundamental building blocks in digital integrated circuits. Nearly all complex digital systems are constructed using combinations of basic logic gates, with the inverter serving as the simplest and most widely used component.

Studying the CMOS inverter provides valuable insight into:

* Transistor-level circuit design
* Layout development techniques
* Design rule verification
* Parasitic extraction
* Timing and power characterization

The inverter consists of:

* A PMOS transistor connected to VDD
* An NMOS transistor connected to GND

When the input changes state, one transistor turns ON while the other turns OFF, producing the complementary output.

---

## Layout Design Using Magic

Magic is an open-source VLSI layout editor used for physical design visualization and verification.

Magic enables designers to:

* Draw transistor-level layouts
* Verify design rule compliance
* Extract parasitic information
* Generate SPICE netlists
* Inspect physical geometries

The layout created in Magic represents the actual physical structure that will eventually be fabricated on silicon.

---

## Importance of Design Rules

Semiconductor fabrication processes impose strict geometric constraints that must be followed to ensure manufacturability and yield.

Common design rule categories include:

### Minimum Width

Defines the smallest allowable width for a layer.

### Minimum Spacing

Specifies the minimum distance required between adjacent features.

### Enclosure Rules

Ensure one layer adequately surrounds another layer.

Violation of these rules may lead to fabrication defects or circuit failures.

---

## SPICE-Based Characterization

After layout creation, circuit performance is evaluated through simulation using ngspice.

Key parameters analyzed include:

### Propagation Delay

Time required for a signal transition to propagate through the inverter.

### Rise Time

Duration required for the output to transition from logic LOW to logic HIGH.

### Fall Time

Duration required for the output to transition from logic HIGH to logic LOW.

### Power Consumption

Measures the energy consumed during switching operations.

Characterization data is later used in standard-cell timing libraries.

---

## Task 1: Clone the Custom Inverter Design Repository

A custom inverter layout repository is cloned to access the inverter design files and supporting resources.

### Repository Cloning Commands

*(Insert Screenshot Here)*

---

## Task 2: Load the Inverter Layout in Magic

After cloning the repository, open the inverter layout using Magic and inspect the device structure.

### CMOS Inverter Layout

*(Insert Screenshot Here)*

### Device Identification

The layout contains:

* PMOS transistor in the N-well region
* NMOS transistor in the P-substrate region
* Input and output routing structures
* Power and ground connections

This physical implementation represents the transistor-level realization of a CMOS inverter.

---

## Task 3: Extract the SPICE Netlist from Magic

Once the layout is verified, generate the corresponding SPICE netlist.

The extraction process converts the physical layout into a circuit representation that can be simulated.

### Extraction Commands

*(Insert Screenshot Here)*

### Generated SPICE File

*(Insert Screenshot Here)*

### Running ngspice

*(Insert Screenshot Here)*

The extracted SPICE netlist includes:

* Transistor parameters
* Connectivity information
* Parasitic capacitances
* Device dimensions

---

## Task 4: Modify the SPICE File for Simulation Analysis

To perform transient analysis, the extracted SPICE file is edited by adding:

* Voltage sources
* Input pulse definitions
* Simulation commands
* Measurement directives

### Modified SPICE File

*(Insert Screenshot Here)*

### Grid Configuration Command

*(Insert Screenshot Here)*

The updated file prepares the inverter for transient simulation and timing characterization.

---

## Task 5: Perform Post-Layout Simulation

After configuring the SPICE model, execute post-layout simulation using ngspice.

### Simulation Commands

*(Insert Screenshot Here)*

### Generated Waveforms

*(Insert Screenshot Here)*

The waveform analysis provides information regarding rise time, fall time, and propagation delay.

---

## Fall Transition Time Calculation

From the output waveform:

* Output reaches 20% of VDD (0.66 V) at:
```math
[
t_1 = 2.122\ ns
]
```
* Output reaches 80% of VDD (2.64 V) at:
```math
[
t_2 = 2.249\ ns
]
```
Therefore,
```math
[
\text{Fall Transition Time} = t_2 - t_1
]
```
```math
[
= 2.249 - 2.122
]
```
```math
[
= 0.127\ ns
]
```
### Fall Time Measurement

*(Insert Screenshot Here)*

---

## Rise Delay Calculation

From the transient response:
```math
[
t_{delay}=2.214-2.149
]
```
```math
[
=0.065\ ns
]
```
This value represents the rise delay of the inverter under the simulated conditions.

### Rise Delay Measurement

*(Insert Screenshot Here)*

---

## Task 6: Investigate and Correct DRC Issues in the Sky130 Technology File

The next exercise focuses on identifying inaccuracies in the older Sky130 Magic technology file and implementing the required corrections.

The objective is to:

* Examine existing DRC behavior
* Identify missing rule checks
* Update technology definitions
* Validate corrected DRC functionality

---

### Commands Used for DRC Testing

*(Insert Screenshot Here)*

### Magic Configuration File

*(Insert Screenshot Here)*

---

## Poly.9 Rule Verification

An intentionally incorrect layout structure is created to test whether the technology file correctly detects a spacing violation.

### Incorrect Poly Spacing Example

*(Insert Screenshot Here)*

Although the spacing is smaller than the required limit of 0.48 μm, the original technology file does not flag a violation.

---

### DRC Violation Detection

*(Insert Screenshot Here)*

---

### Design Rule Investigation

*(Insert Screenshot Here)*

The corresponding technology rules are examined to determine the source of the missing DRC check.

---

## Updating the Technology File

Required modifications are added to the Sky130 technology file to properly enforce the missing rules.

### Added Rule Definitions

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

These modifications enable Magic to correctly identify previously undetected violations.

---

## Validation of Updated Rules

After updating the technology file, multiple test structures are examined to confirm correct DRC operation.

### Validation Results

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

The updated technology file successfully detects the intended violations and confirms correct implementation of the design rules.

---

## Day 3 Summary

### Key Takeaways

* Studied the operation and structure of a CMOS inverter.
* Created and analyzed transistor-level layouts using Magic.
* Learned layout extraction techniques for SPICE simulation.
* Performed post-layout characterization using ngspice.
* Calculated rise delay and fall transition time from transient waveforms.
* Explored design rule verification methodologies.
* Investigated DRC inconsistencies within the Sky130 technology file.
* Implemented and validated corrections to improve rule checking accuracy.
* Gained practical experience in custom standard-cell development and characterization.

## Day 4 – Pre-Layout Timing Analysis and the Importance of Clock Tree Synthesis

### Introduction

Timing analysis is one of the most critical aspects of digital VLSI design. Even if a circuit is functionally correct, it must also satisfy timing constraints to operate reliably at the target clock frequency.

This section focuses on:

* Fundamentals of Static Timing Analysis (STA)
* Setup and hold timing requirements
* Clock tree synthesis (CTS)
* Timing optimization techniques
* Integration of a custom standard cell into the OpenLANE flow

---

## Static Timing Analysis (STA)

Static Timing Analysis is a method used to verify timing requirements without applying simulation vectors. STA evaluates all possible timing paths in the design and determines whether they satisfy specified timing constraints.

The major objectives of STA are:

* Detect setup violations
* Detect hold violations
* Identify critical paths
* Verify timing closure

Unlike dynamic simulation, STA provides complete timing coverage and is significantly faster for large designs.

---

## Important Timing Parameters

### Setup Time

Setup time is the minimum duration for which the input data must remain stable before the active clock edge.

Failure to satisfy setup time results in incorrect data capture by the flip-flop.

### Hold Time

Hold time is the minimum duration for which the input data must remain stable after the active clock edge.

Violating hold time can lead to metastability and unpredictable behavior.

### Clock-to-Q Delay

Clock-to-Q delay represents the time required for a flip-flop output to respond after the arrival of a clock edge.

This delay directly contributes to the timing of downstream logic.

### Slack

Slack measures the difference between the required arrival time and the actual arrival time of a signal.
```math
[
Slack = Required\ Time - Arrival\ Time
]
```
Interpretation:

* Positive Slack → Timing met
* Zero Slack → Timing exactly met
* Negative Slack → Timing violation

---

## Pre-Layout Timing Analysis

Pre-layout timing analysis is performed before placement and routing.

Since actual routing information is unavailable at this stage, estimated interconnect delays are used.

Benefits include:

* Early detection of timing bottlenecks
* Faster design optimization
* Improved timing closure
* Reduced iterations during physical design

Although approximate, pre-layout analysis provides valuable insight into overall design performance.

---

## Clock Tree Synthesis (CTS)

Clock Tree Synthesis is the process of constructing a balanced clock distribution network.

The objective is to deliver the clock signal to all sequential elements with minimal skew and latency.

CTS typically involves:

* Buffer insertion
* Clock inverter insertion
* Clock path balancing
* Clock network optimization

A properly designed clock tree is essential for high-performance synchronous systems.

---

## Clock Skew and Clock Latency

### Clock Skew

Clock skew refers to the difference in clock arrival times at two sequential elements.

Excessive skew can cause:

* Setup violations
* Hold violations
* Timing instability

### Clock Latency

Clock latency represents the total propagation delay from the clock source to a register.

Reducing latency improves overall timing performance and synchronization.

---

## Importance of Clock Tree Synthesis

An optimized CTS implementation provides:

* Reduced clock skew
* Improved timing closure
* Better setup and hold margins
* Enhanced design reliability
* Stable synchronous operation

---

## Task 1: Verify Custom Inverter Cell Before Integration

Before integrating the custom inverter into the design flow, several physical requirements must be satisfied.

### Required Conditions

#### Condition 1

Input and output pins must align with routing track intersections.

#### Condition 2

The standard cell width should be an odd multiple of the horizontal routing pitch.

#### Condition 3

The standard cell height should be an even multiple of the vertical routing pitch.

### Routing Track Information

*(Insert Screenshot Here)*

### Grid Alignment Verification

*(Insert Screenshot Here)*

### Conditions Successfully Verified

*(Insert Screenshot Here)*

---

## LEF Generation

After validating the layout, generate a LEF (Library Exchange Format) file.

The LEF contains:

* Pin information
* Cell dimensions
* Routing obstruction details

### Generated LEF File

*(Insert Screenshot Here)*

---

## Integrating the Custom Inverter into OpenLANE

Copy the required files into the design directory and update the OpenLANE configuration.

### Copy Commands

*(Insert Screenshot Here)*

### Configuration Updates

*(Insert Screenshot Here)*

---

## Re-running Synthesis with Custom Cell

Execute synthesis after introducing the custom inverter.

### Synthesis Execution

*(Insert Screenshot Here)*

### Initial Synthesis Results

*(Insert Screenshot Here)*

Timing violations may appear due to the newly introduced cell characteristics.

---

## Timing Optimization

Design parameters are adjusted to improve timing performance.

### Parameter Modification Commands

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

---

## Placement with Updated Configuration

### Placement Execution

*(Insert Screenshot Here)*

### Merged LEF Verification

*(Insert Screenshot Here)*

### Successful Synthesis Results

*(Insert Screenshot Here)*

### Using Overwrite Tag

*(Insert Screenshot Here)*

### Improved Timing Results

*(Insert Screenshot Here)*

---

## Placement Visualization

### Placement DEF in Magic

*(Insert Screenshot Here)*

### Internal Cell Structures

*(Insert Screenshot Here)*

---

## Timing Analysis and Slack Investigation

### Timing Violation Report

*(Insert Screenshot Here)*

### Fanout Analysis

*(Insert Screenshot Here)*

### Slack Report

*(Insert Screenshot Here)*

### Custom SDC Creation

*(Insert Screenshot Here)*

---

## STA Analysis Reports

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

---

## Timing ECO Optimization

A gate with insufficient drive strength can negatively impact timing performance.

### OR Gate Driving Multiple Fanouts

*(Insert Screenshot Here)*

### Drive Strength Optimization

*(Insert Screenshot Here)*

### Updated STA Reports

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

### Timing Improvement Results

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

---

## Netlist Update and Re-Implementation

After ECO optimization, replace the original synthesized netlist with the improved version.

### Backup Existing Netlist

*(Insert Screenshot Here)*

### Write Verilog Commands

*(Insert Screenshot Here)*

### Updated Netlist Verification

*(Insert Screenshot Here)*

---

## Re-run Physical Design Flow

### Synthesis

*(Insert Screenshot Here)*

### Floorplanning

*(Insert Screenshot Here)*

### Placement

*(Insert Screenshot Here)*

### Clock Tree Synthesis

*(Insert Screenshot Here)*

### CTS Completion

*(Insert Screenshot Here)*

---

## Post-CTS Timing Analysis

After clock tree generation, perform timing analysis to verify timing closure.

### Post-CTS Reports

*(Insert Screenshot Here)*

### OpenROAD Commands

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

### Timing Reports

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

---

## Clock Buffer Experiments

To evaluate CTS behavior, remove selected clock buffers from the CTS buffer list and rerun timing analysis.

### Modified CTS Configuration

*(Insert Screenshot Here)*

### Timing Reports

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

---

## Day 4 Summary

### Key Takeaways

* Learned the fundamentals of Static Timing Analysis.
* Understood setup time, hold time, clock latency, and slack.
* Explored Clock Tree Synthesis methodologies.
* Integrated a custom inverter into the OpenLANE design flow.
* Generated LEF files for custom standard cells.
* Performed timing optimization using ECO techniques.
* Conducted pre-CTS and post-CTS timing analysis.
* Evaluated clock network performance through CTS experiments.

## Day 5 – Final RTL-to-GDSII Implementation Using TritonRoute and OpenSTA

### Introduction

The final stage of the ASIC implementation flow focuses on routing, verification, and sign-off analysis.

At this point:

* Placement has been completed
* Clock tree has been synthesized
* Timing has been optimized

The remaining objective is to create a manufacturable physical layout and verify that it satisfies all design requirements.

---

## Routing

Routing establishes electrical connections between all placed standard cells.

The routing process transforms logical netlists into physical interconnect structures using available metal layers.

---

## Global Routing

Global routing determines the approximate routing paths throughout the chip.

Its objectives include:

* Estimating routing resources
* Avoiding congestion
* Defining routing guides
* Improving routability

Global routing does not generate actual wires but creates routing plans for detailed routing.

---

## Detailed Routing

Detailed routing generates the final physical wires.

This stage performs:

* Metal layer assignment
* Via insertion
* Exact wire placement
* Design rule compliance checks

OpenLANE uses TritonRoute to perform detailed routing.

The resulting layout must be DRC-clean before fabrication.

---

## Sign-Off Verification

Before manufacturing, the design undergoes several verification checks.

---

## Design Rule Check (DRC)

DRC ensures that the layout satisfies all fabrication constraints defined by the Sky130 technology.

Examples include:

* Minimum spacing rules
* Minimum width rules
* Enclosure requirements

A DRC-clean design is mandatory for tape-out.

---

## Layout Versus Schematic (LVS)

LVS verifies consistency between:

* Physical layout
* Circuit schematic/netlist

LVS confirms:

* Correct connectivity
* Proper device implementation
* No missing components

---

## Static Timing Analysis (STA)

Final STA validates that all timing paths satisfy design constraints.

OpenSTA is used to:

* Verify setup timing
* Verify hold timing
* Analyze critical paths
* Confirm timing closure

---

## Power Distribution Network (PDN) Generation

After CTS completion, generate the power delivery network.

The PDN distributes:

* VDD
* Ground

across the entire chip.

### PDN Generation

*(Insert Screenshot Here)*

### PDN Visualization in Magic

*(Insert Screenshot Here)*

### Commands Used

*(Insert Screenshot Here)*

---

## DRC Verification

### Verification Results

*(Insert Screenshot Here)*

The design successfully completes verification without any reported violations.

---

## Routing Execution

Perform routing using TritonRoute.

### Routing Results

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

---

## Routed Layout Analysis

### Routed DEF Visualization

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

### FastRoute Guide

*(Insert Screenshot Here)*

---

## Post-Route Timing Analysis

Following routing, perform timing verification using OpenROAD and OpenSTA.

### Timing Analysis Commands

*(Insert Screenshot Here)*

### Timing Reports

*(Insert Screenshot Here)*

*(Insert Screenshot Here)*

The reports confirm timing behavior after routing parasitics have been included.

---

## Final Deliverables

After successful completion of routing and sign-off verification, the final outputs are generated.

### GDSII File

The GDSII database contains the complete geometric description of the chip layout and serves as the final input for fabrication.

### Fabrication-Ready Design

The completed design satisfies:

* Functional correctness
* Timing requirements
* Physical verification rules
* Manufacturing constraints

This marks the successful completion of the RTL-to-GDSII ASIC implementation flow.

---

## Day 5 Summary

### Key Takeaways

* Learned the concepts of global and detailed routing.
* Performed routing using TritonRoute.
* Generated and analyzed the Power Distribution Network.
* Conducted post-route timing analysis using OpenSTA.
* Verified design integrity through DRC and LVS methodologies.
* Generated a fabrication-ready GDSII layout.
* Completed the entire RTL-to-GDSII implementation flow using OpenLANE and Sky130.

---

# Conclusion

This project provided comprehensive exposure to the complete ASIC design flow using open-source tools and the Sky130 Process Design Kit.

Throughout the five-day workshop, the following stages were explored:

* RTL design and synthesis
* Floorplanning
* Placement
* Standard-cell design
* Timing analysis
* Clock Tree Synthesis
* Routing
* Physical verification
* GDSII generation

The hands-on exercises strengthened both theoretical understanding and practical implementation skills required for modern digital VLSI design.

---

# Acknowledgements

Special thanks to:

**Kunal Ghosh**
Co-Founder, VSD Corp. Pvt. Ltd.

**Nickson P Jose**
Physical Design Engineer, Intel Corporation

**R. Timothy Edwards**
Senior Vice President, Analog and Design, Efabless Corporation

for their valuable guidance, technical expertise, and contributions to the open-source semiconductor ecosystem.
