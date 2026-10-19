# Assignment 10 — SoC Integration and PID Motor Control

The final assignment combined the FPGA motor-control system from
Assignment 8 with a software PID controller running on the Zynq
ARM processor on the ZedBoard.

The focus was hardware–software integration: adapting the existing
VHDL system, connecting it to the processing system and running the
supplied controller on physical hardware.

## Final Lab Demonstration: Integrated PID Control

This is the culmination of the coursework: a fully integrated hardware adaptation. The system successfully runs the supplied C-based PID application on the Zynq SoC, communicating flawlessly with our custom FPGA logic. The video below demonstrates the complete, closed-loop system in action. 

[🎬 Watch the Final System Demonstration](./motor_O10.mp4)

## Assignment Overview

The assignment involved:
- Adapting the motor-control hardware from Assignment 8.
- Packaging the FPGA subsystem as custom IP.
- Connecting the subsystem to the Zynq processing system using AXI GPIO.
- Building the hardware design in Vivado.
- Exporting the hardware platform to Vitis.
- Building and running the supplied C PID application.
- Demonstrating serial communication and PID-controlled motor operation.

## System Architecture

The system divides responsibility between software and FPGA logic:

| Part | Responsibility |
| --- | --- |
| Zynq processing system | Runs the supplied C PID controller |
| AXI GPIO | Exchanges control and feedback values between software and hardware |
| FPGA motor-control subsystem | Generates motor-control signals and processes encoder feedback |
| Serial connection | Reports application output to the host computer |

The software uses velocity feedback to calculate an updated motor
command. The FPGA subsystem applies that command and supplies
feedback for subsequent control updates.

Unlike Assignment 8's self-test command sequence, the motor command
in this assignment comes from the processing system.

## Connection to Earlier Work

This assignment built on the modular motor-control design from
Assignment 8, including its reuse of the PDM component from
Assignment 7.

The progression was from developing individual hardware components,
to integrating an FPGA system, to connecting that system to software
running on an embedded processor.

## Development Workflow

The assignment followed the Vivado-to-Vitis workflow:

1. Adapt the VHDL subsystem's external interface.
2. Package the subsystem as custom IP.
3. Create the processor and peripheral block design in Vivado.
4. Generate the hardware and export its platform.
5. Create the corresponding platform and application in Vitis.
6. Build and run the supplied PID software.
7. Observe serial output and demonstrate the motor-control system.

The original task documents provide the detailed course-specific
instructions.

## Repository Contents

| Path | Description |
| --- | --- |
| [`src/`](src/) | Source material for the assignment |
| [`PID_component/`](PID_component/) | Archived PID application component |
| [`images/`](images/) | Figures accompanying the task documentation |
| [`taskdescription.md`](taskdescription.md) | Original assignment overview and approval requirements |
| [`intro.md`](intro.md) | Introduction to the hardware–software system |
| [`theory.md`](theory.md) | PID background and supplied calculation approach |
| [`vhdl.md`](vhdl.md) | Required changes to the FPGA subsystem |
| [`IP_creation.md`](IP_creation.md) | Custom IP packaging instructions |
| [`blockdesign.md`](blockdesign.md) | Processor and peripheral integration instructions |
| [`build.md`](build.md) | Vitis build, programming and serial-monitoring instructions |

## Practical Verification

The completed system was demonstrated to a lab supervisor and passed.

The assignment's required demonstration covered:
- Working serial communication with the Zynq processor.
- Correct motor operation using PID control.

This records the outcome of the original coursework demonstration.
It does not claim exhaustive testing, optimal PID tuning or a
recent rebuild of the archived files.

## What I Would Do Today

- Record the exact Vivado and Vitis versions used.
- Automate hardware-project recreation where practical.
- Document the hardware–software interface and value scaling.
- Save serial logs and measured control responses alongside the source.

## Learning Value

The main learning outcomes were:
- Integrating custom FPGA logic with an embedded processor.
- Understanding the boundary between hardware and software.
- Using AXI GPIO without implementing the bus protocol directly.
- Moving a hardware platform from Vivado into Vitis.
- Bringing up and demonstrating a complete system on physical hardware.

The central lesson was that a working SoC depends on consistent
interfaces across HDL, processor peripherals, software and hardware.

## Attribution and Archive Status

The PID application was supplied by the course. The assignment
instructions and integration tutorials were also course resources,
partly based on AMD/Xilinx documentation.

My work focused on adapting and integrating the existing FPGA
subsystem, completing the hardware–software workflow and
demonstrating the resulting system.

This directory preserves historical coursework and accompanying
course material. Later README updates document the work rather
than rewrite the original implementation.

The original practical demonstration passed; the archived project
has not been rebuilt or rerun as part of this documentation update.
