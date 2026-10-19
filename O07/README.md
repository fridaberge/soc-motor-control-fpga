# Assignment 7 — Diagrams and Pulse Density Modulation

A learning exercise completed during my degree, connecting digital
design diagrams with a VHDL implementation and a cocotb testbench.

The assignment focused on describing a pulse density modulation
(PDM) module through datapath, block and ASMD diagrams before
implementing and testing the design.

> **Archived coursework**
>
> The original source and supporting files are preserved.
> This README adds context and retrospective reflection rather
> than presenting a corrected or newly validated implementation.

## Assignment Overview

| Part | Focus |
| --- | --- |
| a | Draw the datapath of a simple accumulator-based PDM circuit |
| b | Show the module's connections in a block diagram |
| c | Describe the control behaviour using an ASMD diagram |
| d | Write a cocotb testbench |
| e | Implement the module in synthesizable VHDL and simulate it |

The main objective was to practise creating and interpreting diagrams,
particularly an ASMD diagram that followed the specified behaviour.

A fully working implementation was not required for this assignment;
the module was intended as preparation for the following motor-control
exercise.

## Original Documentation

- [Assignment overview and delivery checklist](taskdescription.md)
- [Detailed tasks and module requirements](task.md)
- [Pulse density modulation background](pulse-density.md)

## Repository Contents

| Path | Description |
| --- | --- |
| [`src/pdm.vhd`](src/pdm.vhd) | Archived PDM implementation |
| [`test/tb_pdm.py`](test/tb_pdm.py) | Stimulus generation and monitoring routines |
| [`images/`](images/) | Images accompanying the documentation |
| [`taskdescription.md`](taskdescription.md) | Original assignment overview |
| [`task.md`](task.md) | Detailed design and verification requirements |
| [`pulse-density.md`](pulse-density.md) | Course-provided background material |

## Intended Module Behaviour

The module accepts a modulation setpoint and timing limits through
`min_on`, `min_off` and `max_on`.

An accumulator supplies carry events to a pulse-duration counter,
while a separate timer limits pulse length and enforces an off interval.

The measurement interface uses `mea_req` and `mea_ack` to request
and acknowledge a pause in pulse generation.

These describe the intended behaviour, not independently verified
properties of the archived implementation.

## Implementation Approach

The VHDL defines three states:

- `pulse_low` — waits for the conditions needed to begin a pulse
  or enter measurement mode.
- `pulse_high` — produces a pulse until the timer or duration
  counter reaches its stopping condition.
- `measure` — acknowledges the measurement request until it
  is released.

The design includes an accumulator, a pulse-duration counter and
a timer, with separate sections for register updates, state
transitions and next-value calculations.

The implementation remains an archived development version rather
than a verified reusable component.

## Original Testing Approach

The testbench contains routines for monitoring:
- Maximum pulse duration
- Time between pulses
- Measurement acknowledgement
- Observed duty cycle

Its main test attempts to apply 50 random setpoints, holding them
for alternating groups of two and four pulse intervals, followed
by five measurement requests.

However, the pulse-timing and duty-cycle assertions are commented
out, and the measurement checker is not started.

The final “All tests passed” log message therefore does not establish
that the required properties were checked successfully.

## What I Would Do Today

- Use `process(all)` for combinational logic and calculate accumulator next-values separately from clocked register updates.
- Correct the decrement guard so the duration counter cannot underflow or decrease while an accumulator carry is present.
- Assign explicit output defaults so pulse generation and measurement acknowledgement are defined in every state.
- Restore the assertions and check minimum off-time against `min_off`, with separate checks for pulse and acknowledgement behaviour.
- Add seeded random stimuli, directed boundary cases and timeouts so missing pulse transitions cannot stall the test indefinitely.
- Compare the diagrams, implementation and test results before describing the module as ready for reuse.

## Learning Value

This exercise connected:
- Datapath and block diagrams
- Algorithmic state-machine design
- Accumulator-based modulation
- Timing constraints and measurement handshakes
- VHDL state-machine implementation
- Concurrent testbench monitoring

The central lesson was that diagrams, code and tests describe the same
system from different perspectives, and their assumptions must agree.

## Attribution and Verification Status

The assignment instructions and explanatory material were supplied
as course resources. This directory preserves my coursework developed
from those materials.

The simulations have not been rerun as part of this documentation
update, and the submitted diagrams have not been reviewed here.

This overview does not claim lab approval, successful synthesis or
functional correctness. The retrospective lists changes I would make
today; those changes have not been applied to the archived code.
