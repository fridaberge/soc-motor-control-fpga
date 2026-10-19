# FPGA and SoC Motor Control

Coursework from **IN3160 – Digital System Design** at the
**University of Oslo**, covering digital hardware design and embedded systems.

The repository includes assignments associated with motor control on a
ZedBoard/Zynq platform using VHDL, C and PID control.

This is a coursework archive. Each assignment folder contains the available
source files and supporting material from that stage of the work.


## Contents
## Technologies and Tools
## Assignment Overview

### Assignment 1 — Introduction to FPGA Design and Simulation

A guided introduction to Vivado, VHDL and testbench-based simulation. The archived example implements a four-bit up/down counter with synchronous reset, parallel loading and boundary flags, accompanied by a cocotb testbench and GHDL simulation configuration.

The original assignment guide is unavailable. The testbench provides input stimulus but does not include automated output assertions.

[Implementation, simulation instructions and limitations](oblig1/README.md)

### Assignment 2 — Combinational Decoder in VHDL

Implemented a two-input, four-output active-low decoder using two alternative VHDL description styles: `case` and `with … select`.

The assignment separates the shared entity from the alternative architectures and includes cocotb testbenches, a GHDL simulation configuration and an archived waveform screenshot.

The original task description is unavailable. The archived testbench assertions require correction before automated verification can be relied upon.

[Implementation, simulation instructions and limitations](assignment2/README.md)

### Assignment 3 — Signals, Variables and Process Sensitivity

A VHDL learning exercise investigating variable and signal assignments,
clocked behaviour and process sensitivity lists through simulation,
written explanations and a draw.io diagram.

The original submission is preserved, including mistakes. An accompanying
retrospective discusses what I now recognise about simulation semantics,
timing units and diagram notation, and how I would approach the exercise
differently.

[Original work and retrospective](O03/readme.md)

### Assignment 4 — Structural Shift Registers

Built shift registers from a supplied flip-flop component, progressing
from eight explicit instances to a generate-based 32-bit design and
a generic-width implementation.

The archived VHDL and cocotb testbenches are accompanied by a retrospective
on interface completeness, bit-order conventions and verification.

[Original work and retrospective](O04/readme.md)

### Assignment 5 — Functions, Packages and Self-Checking Verification

Refactored a supplied parity generator into reusable VHDL functions
and a package, then extended a cocotb testbench with generated inputs
and assertions.

The archived work includes simulation screenshots and a retrospective
on reproducible testing, reset handling and verification coverage.

[Original work and retrospective](O05/readme.md)

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

[Original work and retrospective](O07/readme.md)

### Assignment 8 — FPGA Motor-Control System

Integrated motor-command generation, direction control, synchronization,
quadrature decoding and velocity display.

Reused the PDM component from Assignment 7 after recognising its
connection to the motor-modulation requirements.

[Implementation and retrospective](O08/readme.md)

### Assignment 9 — Pipelined RGB-to-Grayscale Conversion

Developed a two-stage grayscale conversion pipeline from supplied
starting code, focusing on arithmetic sizing, overflow saturation
and alignment of pixel data with validity signals.

The archive includes VHDL, an image-based cocotb testbench and
diagram files.

[Implementation and retrospective](O09/readme.md)

## Motor-Control System
## My Contribution
## Repository Guide
## Building and Running
## Results and Demonstrations
## Limitations and Further Work
## What I Learned
