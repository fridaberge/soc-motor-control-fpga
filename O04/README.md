# Assignment 4 — Structural Shift Registers in VHDL

A learning exercise completed during my degree as part of
**IN3160 – Digital System Design** at the **University of Oslo**.

This assignment explored structural VHDL by building shift registers
from a supplied single-bit flip-flop component. It progressed from
explicit component instantiation to generate statements and a
generic-width design.

> **Archived coursework**
>
> The original source files and testbenches are preserved as coursework.
> This page adds context and retrospective reflection; it does not
> represent a corrected or newly validated implementation.

## Assignment Overview

The task was to reuse the supplied `dff` component without modification
to build three shift registers:

| Part | Component | Task |
| --- | --- | --- |
| a | `shift8` | Instantiate eight flip-flops explicitly |
| b | `shift32` | Build a 32-bit register using a generate statement |
| c | `shiftn` | Generalise the width and simulate a 64-bit instance |

The assignment required a single-bit serial input, both parallel and
serial outputs, and Python testbenches.

The supplied flip-flop uses a rising-edge clock and a synchronous,
active-low reset.

[Read the original assignment instructions](task_description.md).

## Repository Contents

| Path | Description |
| --- | --- |
| [`src/dff.vhd`](src/dff.vhd) | Supplied single-bit flip-flop component |
| [`src/shift8.vhd`](src/shift8.vhd) | Eight explicitly instantiated flip-flops |
| [`src/shift32.vhd`](src/shift32.vhd) | Fixed-width implementation using a generate loop |
| [`src/shiftn.vhd`](src/shiftn.vhd) | Generic-width implementation, defaulting to 64 bits |
| [`test/tb_shift8.py`](test/tb_shift8.py) | Eight-bit testbench |
| [`test/tb_shift32.py`](test/tb_shift32.py) | 32-bit testbench |
| [`test/tb_shiftn.py`](test/tb_shiftn.py) | Testbench targeting the 64-bit configuration |
| [`images/`](images/) | Illustrations referenced by the assignment instructions |
| [`task_description.md`](taskdescription.md) | Original course-provided instructions |

## Implementation Approach

### Explicit Instantiation

In `shift8`, I connected eight instances of `dff` using named port
associations. The input enters bit 7 and moves towards bit 0, which
also drives the serial output.

This made each stage and connection visible, but required repetitive
code.

### Generate Statement

In `shift32`, I instantiated the first stage separately and used a
generate loop for the remaining stages.

In this version, the input enters bit 0 and moves towards bit 31.
The final stage drives the serial output.

### Generic Width

In `shiftn`, I replaced the fixed width with the positive generic `N`.
The internal vector and generate range depend on that value.

The default width is 64 bits, matching the configuration requested
for part c.

## Original Testing Approach

The cocotb testbenches start a clock, apply input data while reset is
active, then release reset and apply a shifting sequence.

The eight-bit test uses a single `1` followed by zeros. The wider
tests use alternating ones and zeros.

The tests include assertions for reset values and the resulting
register contents and serial output.

These checks document my original testing approach. They are not
evidence of a newly verified simulation run.

## Retrospective — What I Would Approach Differently

### Check the External Interface Against the Requirements

The assignment requested both parallel and serial outputs.

My eight-bit implementation exposes both. In the 32-bit and generic
versions, however, `parallel` is an internal signal rather than an
output port.

Although those testbenches attempt to inspect the internal vector,
that is not the same as providing the required external interface.

Today, I would compare each entity's ports against the requirements
before considering the implementation complete.

### Keep the Bit-Order Convention Consistent

The eight-bit version shifts from the highest index towards the
lowest, while the wider versions shift in the opposite index direction.

Different indexing conventions can describe a shift register, but
changing conventions between related implementations makes comparison
and testing harder.

I would now choose one convention and document which end receives
new data and which end produces the serial output.

### Separate Debugging Observations from Language Rules

A source comment records that I switched to an ascending generate
range after encountering difficulty with a descending version.

That captures my debugging experience, but it does not establish
that descending generate ranges are unsupported.

Today, I would investigate the specific range and indexing issue,
then document the actual cause rather than leave a broad conclusion.

### Make Verification More Systematic

The original tests check selected outcomes using a small number of
input patterns.

If repeating the exercise, I would use a reference model to check
the expected register state after every sampled clock cycle. I would
also make input-driving and output-sampling phases explicit.

Additional cases would include:
- All-zero and all-one sequences
- An isolated bit travelling through every stage
- Reset applied during shifting
- Multiple widths for the generic implementation
- The minimum supported width

### Keep Test Messages Consistent

Some comments and assertion messages were copied between tests and
do not match the widths or expected values being checked.

For example, the eight-bit test checks `00000001` while its failure
message describes `10000000`.

I would now derive diagnostic messages from the expected values
instead of maintaining separate handwritten descriptions.

## Learning Value

This exercise introduced a progression from manually connecting
components to describing repeated hardware structures more concisely.

It gave me practice with:
- Structural VHDL and component reuse
- Named port association
- Generate statements
- Generic parameters
- Clocked data movement and synchronous reset
- Python-based simulation tests

Looking back, it also highlights that reusable code needs consistent
interfaces, clear indexing conventions and verification that checks
more than a final result.

## Attribution and Archive Status

The assignment instructions and `dff` component were provided as
course material. The shift-register implementations and accompanying
testbenches are archived here as my coursework.

The illustrations in `images/` accompany the original instructions;
they are not presented as my own diagrams.

The original implementation has not been changed as part of this
documentation update. The simulations have not been rerun, and the
retrospective describes improvements I would make today rather than
changes already applied.
