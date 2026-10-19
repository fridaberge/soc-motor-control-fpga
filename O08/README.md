# Assignment 8 — FPGA Motor-Control System

Coursework exploring modular FPGA design through motor-control
signal generation, encoder feedback and velocity display.

The exercise brought together state machines, synchronization,
signed arithmetic and components developed in earlier assignments.

## Assignment Overview

The assignment involved:
- Generating a sequence of signed motor commands from a ROM.
- Implementing motor modulation and direction control.
- Synchronizing external inputs and motor-control outputs.
- Decoding quadrature encoder transitions.
- Connecting a supplied velocity reader to a seven-segment display.
- Integrating the modules into a larger FPGA system.

## Implementation Approach

I initially started a separate pulse-width modulator, then recognised
that the PDM component from Assignment 7 could be reused.

After discussing this with the professor, who confirmed that
recognising this connection was an intended learning outcome,
I continued with the PDM-based approach.

The retained implementation uses `pwm2` to combine signed-command
handling and direction control with a `pdm` submodule.

This was an important lesson in recognising reusable functionality
rather than implementing a similar component again.

## System Structure

The motor-command path in `system_test.vhd` connects:

- `self_test` — supplies the command sequence.
- `pwm2` — handles signed commands and direction control.
- `pdm` — generates pulses inside `pwm2`.
- `output_synch` — registers the direction and enable outputs.

The feedback path in `system_quad.vhd` connects:

- `input_synch` — synchronizes the encoder inputs.
- `quad` — produces position increment and decrement pulses.
- `velocity_reader` — calculates signed velocity.
- `seg7ctrl` — displays the magnitude of the velocity value.

The top-level `system.vhd` instantiates both subsystem wrappers.
These connections describe the archived source structure, not
independently verified system behaviour.

## Key Files

| Path | Role |
| --- | --- |
| [`src/system.vhd`](src/system.vhd) | Top-level integration |
| [`src/system_test.vhd`](src/system_test.vhd) | Motor-command subsystem |
| [`src/pwm2.vhd`](src/pwm2.vhd) | Direction control and PDM integration |
| [`src/pdm.vhd`](src/pdm.vhd) | Pulse density modulation |
| [`src/system_quad.vhd`](src/system_quad.vhd) | Encoder and display subsystem |
| [`test/`](test/) | Testbench and simulation material |

## Original Documentation

- [Assignment overview](taskdescription.md)
- [Introduction](intro.md)
- [Design process and delivery requirements](process.md)
- [Motor modulation requirements](pwm.md)
- [Synchronization modules](synch.md)
- [Quadrature decoder](quad.md)

The directory also contains the supplied encoder and motor-driver
reference documents.

## Testing Context

The supplied Makefile selects `system` as the simulation top level
and `tb_system` as the Python test module.

It also contains commented alternatives for testing individual
parts of the design.

This identifies the configured simulation target; it does not
establish that the tests passed.

## What I Would Do Today

- Make combinational sensitivity lists and register boundaries explicit.
- Test zero, signed endpoints and direction reversals separately.
- Verify each subsystem before running the integrated system test.
- Use explicit source lists so abandoned drafts cannot affect compilation.
- Keep reproducible simulation results and hardware reports with the source.

## Learning Value

The main learning outcomes were:
- Recognising opportunities to reuse an existing component.
- Separating modulation from direction control.
- Connecting synchronized inputs to state-machine logic.
- Integrating independently developed modules.
- Distinguishing source-level integration from verified operation.

## Attribution and Archive Status

The assignment instructions, hardware references, PWM testbench
and velocity reader were supplied as course resources.

The archive contains my coursework alongside those materials;
not every file is original work authored by me.

Later documentation and any removal of abandoned drafts are
repository-maintenance changes rather than a rewrite of the
retained coursework implementation.

Compilation, simulation and hardware operation have not been
reverified as part of this documentation update.
