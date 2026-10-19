# Assignment 5 — Functions, Packages and a Self-Checking Testbench

A learning exercise completed during my degree, exploring VHDL
functions, reusable packages and automated verification with cocotb.

The assignment involved refactoring a supplied parity generator and
extending its testbench with generated inputs and automatic checks.

> **Archived coursework**
>
> The original source files, testbench and screenshots are preserved.
> This README adds context and retrospective reflection rather than
> presenting a corrected or newly validated implementation.

## Assignment Overview

The exercise was divided into five parts:

| Part | Focus |
| --- | --- |
| a | Simulate the supplied parity generator and capture a waveform |
| b | Encapsulate two parity-calculation methods in separate functions |
| c | Move the functions into a reusable VHDL package |
| d | Generate random inputs alongside a counting input |
| e | Add assertions checking the internal parity signals and output |

The assignment also included optional questions about synthesis
and FPGA architecture. This overview does not claim completion of
those optional investigations.

[Read the original assignment instructions](taskdescription.md).

## Repository Contents

| Path | Description |
| --- | --- |
| [`src/pargen.vhd`](src/pargen.vhd) | Parity generator using the packaged functions |
| [`src/subprog_pck.vhd`](src/subprog_pck.vhd) | Package containing two parity functions |
| [`test/tb_pargen.py`](test/tb_pargen.py) | Stimulus generation, prediction and assertions |
| [`taska.png`](taska.png) | Archived screenshot associated with part a |
| [`taskd.png`](taskd.png) | Archived screenshot associated with part d |
| [`taske.png`](taske.png) | Archived screenshot associated with part e |
| [`taskdescription.md`](taskdescription.md) | Original course-provided instructions |

## Implementation

### Parity Functions

The package `subprog_pck` contains two approaches:

- `method_1` loops over the input vector and toggles a variable for
  each bit equal to `1`.
- `method_2` uses the VHDL-2008 reduction XOR operator.

Both functions accept an unconstrained `std_logic_vector`.

### Parity Generator

The generator defaults to two 16-bit inputs.

It applies the toggle method to `indata1` and the reduction XOR
method to `indata2`. Their results drive the internal signals
`toggle_parity` and `xor_parity`.

The two results are combined using XOR. The output `par` captures
the combined value on a rising clock edge, with a synchronous,
active-low reset.

### Self-Checking Testbench

The testbench applies 20 input pairs:

- A randomly generated value for `indata1`
- An incrementing value for `indata2`

Inputs are driven on falling clock edges. A separate comparison
coroutine waits for rising edges and then `ReadOnly()` before
checking the two internal parity signals and the registered output.

The Python helper functions provide expected parity values for
the assertions.

  ## Simulation & Verification Results

To ensure the reliability of the design, comprehensive testbenches were created. The waveform captures below serve as concrete verification that the internal logic and timing behave exactly as intended under simulated conditions, proving the functional correctness of the code before moving to hardware.

This waveform verifies the bin2ssd module by demonstrating the correct translation of 4-bit binary inputs into 7-segment display codes according to the truth table.
![Simulation Waveform Task a](taska.png)

This waveform validates the integrated self_test_system, confirming that the c and abcdefg outputs correctly cycle through the secret message sequence driven by the internal one-second timer.
![Simulation Waveform Task b](taskd.png)

This waveform demonstrates the optional base-10 counter system, showing how the separate digit counters correctly increment the display in response to simulated button presses.
![Simulation Waveform Task e](taske.png)

## Retrospective — What I Would Approach Differently

### Make Random Tests Reproducible

The original testbench generates random inputs without recording
an explicit seed.

Today, I would use and log a seed so that an unexpected result
could be reproduced with the same input sequence.

I would also review the random range: the current call excludes
`0xFFFF`. Explicit boundary cases would make the intended coverage
clearer than relying on random generation alone.

### Make Reset Handling Clearer

The comparison loop contains calls to `reset_dut(dut)` without
awaiting or scheduling the coroutine. Those calls do not express
a clear reset sequence.

If repeating the exercise, I would keep reset control separate
from the monitor and explicitly account for reset when predicting
the registered output.

I would also make reset assertion and release timing unambiguous
relative to the clock.

### Strengthen the Verification Strategy

Twenty input pairs satisfy the assignment's minimum stimulus
count, but they do not demonstrate comprehensive coverage.

I would now add directed cases such as:
- All zeros and all ones
- A single set bit at each position
- Inputs with known odd and even parity
- Reset during an active input sequence
- Additional supported input widths

The two VHDL methods currently receive different inputs. I would
also test both methods against the same vectors to compare their
results directly.

### Improve Names and Diagnostics

Names such as `method_1` and `method_2` reflect the exercise
structure but do not explain the functions' behaviour.

Today, I would choose descriptive names and include the input
values, expected result and observed result in assertion messages.

I would also remove leftover placeholder comments and unused
declarations.

### Preserve Intermediate Versions

The assignment progressed from the original generator to local
functions and then a package-based implementation.

I would now preserve those stages through clearly identified
commits or separate snapshots. This would make it easier to show
what changed in each part without reconstructing earlier versions.

## What I Learned

This exercise gave me practice with:
- Refactoring VHDL into functions
- Sharing functionality through a package
- Using unconstrained vector parameters
- Generating test inputs in Python
- Running stimulus and monitoring concurrently
- Checking results automatically with assertions
- Considering simulation scheduling when sampling outputs

It also introduced an important distinction: a waveform helps
inspect behaviour, while a self-checking testbench makes expected
behaviour explicit and repeatable.

## Attribution and Verification Status

The assignment was based on supplied course material, including
a parity-generator example and testbench scaffolding. This directory
preserves my coursework modifications to those materials.

The simulations have not been rerun as part of this documentation
update. The retrospective describes improvements I would make
today; those changes have not been applied to the archived code.
