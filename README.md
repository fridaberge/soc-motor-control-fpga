# FPGA and SoC Motor Control

Coursework from **IN3160 – Digital System Design** at the
**University of Oslo**, covering digital hardware design and embedded systems.

The repository follows a progression from introductory VHDL exercises
to an FPGA motor-control system and its integration with a software
PID controller on a ZedBoard/Zynq platform.

It also includes exercises in simulation, verification, modular design
and pipelined image processing.

**Final system:** The Assignment 10 motor-control system was demonstrated
to a lab supervisor and passed the practical assignment verification.

This is a historical coursework archive, not a production-ready
hardware library. Assignment folders contain the available source files,
course resources and supporting material from that stage of the work,
alongside later documentation and retrospectives.

## Contents

- [Technologies and Tools](#technologies-and-tools)
- [Assignment Overview](#assignment-overview)
- [Motor-Control System](#motor-control-system)
- [My Contribution](#my-contribution)
- [Repository Guide](#repository-guide)
- [Building and Running](#building-and-running)
- [Results and Demonstrations](#results-and-demonstrations)
- [Limitations and Further Work](#limitations-and-further-work)
- [What I Learned](#what-i-learned)

## Technologies and Tools

| Technology or tool | Use in the coursework |
| --- | --- |
| VHDL | Digital hardware description |
| Python and cocotb | Simulation stimulus and testbench checks |
| GHDL | VHDL simulation |
| AMD/Xilinx Vivado | FPGA design, IP packaging and hardware integration |
| AMD/Xilinx Vitis | Embedded application development and deployment |
| C | Supplied PID application running on the Zynq processor |
| AXI GPIO | Communication between processor software and FPGA logic |
| ZedBoard / Zynq | Hardware platform for the integrated system |
| Make | Simulation configuration and execution |
| draw.io | Datapath and design diagrams |
| Pillow | Image handling in the grayscale-conversion testbench |

The assignments do not all use the same tools. Consult the individual
folder README before attempting to run an exercise.

## Assignment Overview

### Assignment 1 — Introduction to FPGA Design and Simulation

A guided introduction to Vivado, VHDL and testbench-based simulation.
The archived example implements a four-bit up/down counter with
synchronous reset, parallel loading and boundary flags, accompanied
by a cocotb testbench and GHDL simulation configuration.

The original assignment guide is unavailable. The testbench provides
input stimulus but does not include automated output assertions.

[Implementation, simulation instructions and limitations](O01/README.md)

### Assignment 2 — Combinational Decoder in VHDL

Implemented a two-input, four-output active-low decoder using two
alternative VHDL description styles: `case` and `with … select`.

The assignment separates the shared entity from the alternative
architectures and includes cocotb testbenches, a GHDL simulation
configuration and an archived waveform screenshot.

The original task description is unavailable. The archived testbench
assertions require correction before automated verification can be
relied upon.

[Implementation, simulation instructions and limitations](O02/README.md)

### Assignment 3 — Signals, Variables and Process Sensitivity

A VHDL learning exercise investigating variable and signal assignments,
clocked behaviour and process sensitivity lists through simulation,
written explanations and a draw.io diagram.

The original submission is preserved, including mistakes. An accompanying
retrospective discusses what I now recognise about simulation semantics,
timing units and diagram notation, and how I would approach the exercise
differently.

[Original work and retrospective](O03/README.md)

### Assignment 4 — Structural Shift Registers

Built shift registers from a supplied flip-flop component, progressing
from eight explicit instances to a generate-based 32-bit design and
a generic-width implementation.

The archived VHDL and cocotb testbenches are accompanied by a retrospective
on interface completeness, bit-order conventions and verification.

[Original work and retrospective](O04/README.md)

### Assignment 5 — Functions, Packages and Self-Checking Verification

Refactored a supplied parity generator into reusable VHDL functions
and a package, then extended a cocotb testbench with generated inputs
and assertions.

The archived work includes simulation screenshots and a retrospective
on reproducible testing, reset handling and verification coverage.

[Original work and retrospective](O05/README.md)

### Assignment 6 — Modular Seven-Segment Display Control

Developed a modular VHDL design combining seven-segment decoding,
two-digit multiplexing and a ROM-based message generator.

The archived coursework is accompanied by a retrospective on reset
behaviour, timing definitions, parameterisation and module integration.

[Original work and retrospective](O06/readme.md)

### Assignment 7 — Diagrams and Pulse Density Modulation

Explored diagram-driven digital design through a pulse density
modulation exercise, with an archived VHDL implementation and
cocotb testbench.

The retrospective covers state-machine structure, counter boundaries
and the difference between monitoring signals and actively checking
requirements.

[Original work and retrospective](O07/README.md)

### Assignment 8 — FPGA Motor-Control System

Integrated motor-command generation, direction control, synchronization,
quadrature decoding and velocity display.

Reused the PDM component from Assignment 7 after recognising its
connection to the motor-modulation requirements.

[Implementation and retrospective](O08/README.md)

### Assignment 9 — Pipelined RGB-to-Grayscale Conversion

Developed a two-stage grayscale conversion pipeline from supplied
starting code, focusing on arithmetic sizing, overflow saturation
and alignment of pixel data with validity signals.

The archive includes VHDL, an image-based cocotb testbench and
diagram files.

[Implementation and retrospective](O09/README.md)

### Assignment 10 — SoC Integration and PID Motor Control

Integrated the FPGA motor-control system from Assignment 8 with a
supplied C PID controller running on the Zynq ARM processor.

The assignment covered custom IP packaging, AXI GPIO integration,
the Vivado-to-Vitis workflow and serial monitoring.

**Verification:** Demonstrated the system to a lab supervisor and
passed the practical assignment verification.

[Implementation and retrospective](O10/README.md)

## Motor-Control System

The motor-control work develops across three assignments:

| Assignment | Role in the progression |
| --- | --- |
| 7 | Develop a pulse density modulation component |
| 8 | Reuse that component within a modular FPGA motor-control system |
| 10 | Connect the FPGA subsystem to a software PID controller on Zynq |

Assignment 8 combines two main paths:

- **Command path:** motor-command generation, direction control and
  pulse generation.
- **Feedback path:** synchronized encoder inputs, quadrature decoding,
  velocity measurement and display.

Assignment 10 replaces the self-test command source with commands
from the processor-side controller. Software and FPGA logic exchange
control and feedback values through AXI GPIO.

The division of responsibilities is:

- **Processor software:** execute the supplied PID calculation and
  update the motor command using velocity feedback.
- **FPGA logic:** generate motor-control signals and process encoder
  feedback.
- **Serial connection:** report application output to the host computer.

Assignment 9 is a separate image-processing exercise. It develops
related skills in arithmetic sizing, register placement and control
timing rather than forming part of the motor-control datapath.

## My Contribution

My work progressed from guided exercises and modifications of supplied
examples to integrating a complete hardware–software system.

Across the assignments, my contributions included:
- Writing and adapting VHDL modules.
- Developing structural and modular designs.
- Modifying cocotb testbenches and adding checks.
- Creating diagrams and written design explanations.
- Reusing earlier components in later assignments.
- Integrating the FPGA subsystem with the Zynq processing system.
- Completing the final hardware demonstration.

An important design decision was reusing Assignment 7's PDM component
in Assignment 8. I initially started a separate modulator, then
recognised the connection to the earlier exercise. The professor
confirmed that recognising this opportunity for reuse was an intended
learning outcome.

### Attribution

This repository also contains course-provided material, including
assignment instructions, starter code, testbenches, reference material
and the PID application used in Assignment 10.

I do not claim authorship of every file. The individual assignment
READMEs distinguish supplied material from my work where that
information is available.

The retrospectives are later reflections on the archived coursework,
not claims that the original submissions already followed the
practices discussed there.

## Repository Guide

| Directory | Topic |
| --- | --- |
| [`O01/`](O01/) | Introductory FPGA design and simulation |
| [`O02/`](O02/) | Combinational decoder |
| [`O03/`](O03/) | Signals, variables and process sensitivity |
| [`O04/`](O04/) | Structural shift registers |
| [`O05/`](O05/) | Functions, packages and parity verification |
| [`O06/`](O06/) | Seven-segment display system |
| [`O07/`](O07/) | Diagrams and pulse density modulation |
| [`O08/`](O08/) | FPGA motor-control integration |
| [`O09/`](O09/) | Pipelined grayscale conversion |
| [`O10/`](O10/) | Zynq integration and software PID control |

Assignment directories use consistent numbering from O01 to O10.

Depending on the assignment, a folder may contain:
- VHDL source files.
- Python testbenches and simulation Makefiles.
- Diagrams, waveforms or screenshots.
- Original assignment instructions.
- A README describing the implementation and known limitations.

Where the original assignment overview has been renamed to
`taskdescription.md`, it is kept separately from the later project README.

## Building and Running

There is no single build command for the entire repository.
The exercises use separate simulation configurations, and the final
SoC assignment requires a hardware and embedded-software workflow.

### VHDL Simulation

For assignments with a GHDL/cocotb Makefile:

1. Read the assignment README and its limitations.
2. Install compatible versions of GHDL, Python, cocotb and Make.
3. Inspect the Makefile's source paths, top-level entity and test module.
4. Run `make` from the directory containing that Makefile.

Some exercises require additional Python dependencies, such as Pillow
for image processing.

**Not every archived testbench is self-checking or correct.**
A simulation that runs without errors is not, by itself, proof that
the design satisfies its requirements.

### FPGA and SoC Hardware

Assignment 10 uses Vivado and Vitis to build the hardware platform,
prepare the embedded application and run it on the ZedBoard.

Start with:
- [Assignment 10 overview](O10/README.md)
- [IP packaging instructions](O10/IP_creation.md)
- [Block-design instructions](O10/blockdesign.md)
- [Build and run instructions](O10/build.md)

The archive has not been rebuilt as part of the documentation update.
Tool-version differences, local paths and generated project dependencies
may require attention.

Before operating the motor hardware, check the course instructions,
board constraints, wiring and motor-driver requirements.

## Results and Demonstrations

### Final Hardware Demonstration

The Assignment 10 system was demonstrated to a lab supervisor and passed
the required practical verification.

The demonstration requirements covered:
- Serial communication with the Zynq processor.
- Motor operation using PID control.

This records the original coursework outcome. It does not establish
optimal controller tuning, exhaustive verification or present-day
reproducibility of the archived project.

### Archived Evidence

Some assignment folders contain screenshots, diagrams, waveforms or
other supporting material. Their scope and limitations are described
in the corresponding READMEs.

An archived screenshot or waveform illustrates a particular run;
it should not be treated as a substitute for a reproducible,
self-checking test suite.

## Limitations and Further Work

This repository preserves a learning process, including incomplete
approaches, mistakes and uneven verification.

Known limitations include:
- Missing original task descriptions for the earliest assignments.
- Testbenches with limited assertions or checks requiring correction.
- Inconsistent naming and project organization.
- Tool-version and environment details that are not fully recorded.
- No repository-wide automated verification.
- No recent rebuild or rerun as part of the documentation work.

If revisiting the designs, I would prioritize:
- Reproducible simulation environments and explicit source lists.
- Self-checking tests for reset, boundaries and timing behaviour.
- Clearer interfaces and consistent parameterisation.
- Automated hardware-project recreation where practical.
- Recorded serial logs and measured control responses for the final system.

These are retrospective improvements, not features already implemented
throughout the archive.

## What I Learned

The coursework connected several levels of digital system design:

- **VHDL semantics:** understanding signals, variables, processes and
  the relationship between simulation and hardware.
- **Modular design:** defining interfaces and integrating smaller
  components into larger systems.
- **Verification:** distinguishing stimulus, visual inspection and
  automated requirement checking.
- **Arithmetic and timing:** managing bit growth, overflow, pipeline
  latency and alignment of data with control signals.
- **Reuse:** recognising when an existing component solves a new problem.
- **Hardware–software integration:** coordinating FPGA logic, processor
  peripherals, embedded software and physical hardware.

The main progression was from writing individual modules to reasoning
about a complete system—and learning that successful integration depends
as much on clear interfaces and verification as on the implementation
of each component.
