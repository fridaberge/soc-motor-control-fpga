# Assignment 1 — Introduction to FPGA Design and Simulation

A guided introduction to Vivado, VHDL programming and testbench-based simulation for FPGA design, completed as part of **IN3160 – Digital System Design** at the University of Oslo.

The original assignment guide is not included in this repository. This overview is based on the archived implementation and recollection of the exercise.

## Overview

The exercise introduced the development workflow for digital hardware:
- Describing a circuit in VHDL
- Working with FPGA projects in Vivado
- Creating a testbench to stimulate a design
- Running simulations and inspecting signal behaviour

The archived example implements a four-bit counter with load, reset and counting-direction controls.

## VHDL Implementation

The design in [`src/first.vhd`](src/first.vhd) provides:

| Signal | Direction | Purpose |
| --- | --- | --- |
| `clk` | Input | Clock; the counter updates on rising edges |
| `reset` | Input | Synchronous, active-high reset to zero |
| `load` | Input | Loads the value supplied through `inp` |
| `inp` | Input | Four-bit value to load |
| `up` | Input | Selects upward counting when high and downward counting when low |
| `count` | Output | Current four-bit counter value |
| `max_count` | Output | High when the count is 15 and upward counting is selected |
| `min_count` | Output | High when the count is zero and downward counting is selected |

Reset takes priority over loading and counting. Otherwise, loading takes priority over the selected counting direction.

The counter wraps around at its four-bit boundaries rather than stopping. The minimum and maximum flags indicate the current boundary condition; they do not prevent further counting.

## Testbench

[`tb/tb_first.py`](tb/tb_first.py) uses cocotb to stimulate the VHDL design.

The testbench:
1. Starts a clock with a 20 ns period, corresponding to 50 MHz.
2. Applies and releases reset.
3. Requests a load of the value 10.
4. Selects upward counting.
5. Switches to downward counting.

This is a stimulus-based demonstration rather than a self-checking test. It contains no assertions comparing the outputs against expected values, so completing the simulation does not establish functional correctness.

## Repository Contents

| Path | Contents |
| --- | --- |
| [`src/`](src/) | VHDL source code |
| [`tb/`](tb/) | Python testbench and simulation makefile |
| [`project_1/`](project_1/) | Archived project directory; its contents and role have not yet been documented |
| [`project_2/`](project_2/) | Archived project directory; its contents and role have not yet been documented |

## Running the Simulation

### Requirements

The supplied simulation configuration uses:
- Python with cocotb
- GHDL
- GNU Make
- VHDL-2008 support

The original tool versions have not been recorded, and compatibility with a current installation has not yet been verified.

### Simulation Command

If the simulation configuration is saved as `tb/makefile.txt`, run the following from the repository root with the required tools available:

```bash
cd O01/tb
make -f makefile.txt
```

If the file is instead named `Makefile`, use `make` from the same directory.

The configuration selects:
- `first` as the top-level VHDL entity
- `../src/first.vhd` as the design source
- `tb_first` as the Python test module
- `first.ghw` as the waveform output

Inspect the waveform to examine reset, loading, counting direction and boundary flags.

These instructions describe the supplied configuration and have not yet been reverified in a clean environment.

## Vivado Projects

The exercise also introduced Vivado. However, the project entry points, target device, constraints and hardware-programming procedure have not yet been documented.

The GHDL/cocotb simulation procedure above is separate from opening or building the archived Vivado projects.

## Context and Contribution

This was a guided introductory exercise, not an independently designed FPGA system. The archived files should be understood in that context.

The exact division between supplied example code and personal modifications has not yet been documented.

## Limitations and Further Work

- Recover the original assignment guide, if available.
- Document the contents and purpose of both project directories.
- Record the tool versions and verify the simulation procedure.
- Add assertions for reset, loading, both counting directions and wraparound.
- Verify the timing of testbench input changes relative to clock edges.
- Document any FPGA hardware testing separately from simulation.

## What I Learned

- Basic VHDL entity and architecture structure
- Clocked processes and synchronous reset
- Counter control and combinational next-state logic
- Python-based hardware stimulation with cocotb
- Introductory Vivado project workflow
- The distinction between applying test inputs and automatically checking results
