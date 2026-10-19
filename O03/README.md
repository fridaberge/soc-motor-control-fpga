# Assignment 3 — Signals, Variables and Process Sensitivity

A learning exercise completed during my degree as part of **IN3160 – Digital System Design** at the **University of Oslo**.

This assignment explored how variables, signals and process sensitivity lists affect VHDL simulation behaviour. It combined simulation, written explanations and a diagram created in draw.io.

> **Archived coursework**
>
> The original code, answers and illustration are preserved as submitted.
> They reflect my understanding at the time and contain mistakes.
> The retrospective below explains what I now recognise and what I would
> approach differently.

## Assignment Overview

The exercise was divided into four parts:

| Part | Focus |
| --- | --- |
| a | Simulate a clocked design and explain its output timing |
| b | Replace variables with signals and investigate the resulting behaviour |
| c | Compare variable and signal assignments and draw a conceptual diagram |
| d | Modify a process sensitivity list and explain the effect on simulation |

The original instructions are available in the
[assignment description](task_description.md).

## Repository Contents

| Path | Description |
| --- | --- |
| [`src/`](src/) | Archived VHDL source files |
| [`test/`](test/) | Archived testbenches and simulation configuration |
| [`tasks.md`](tasks.md) | My original written answers |
| [`3c.drawio.png`](3c.drawio.png) | My original draw.io illustration for part c |
| [`task_description.md`](taskdescription.md) | Original course-provided assignment instructions |
| [`tasks_tmp.html`](tasks_tmp.html) | Additional archived HTML file |

## My Work

My contribution included the written explanations in `tasks.md` and
the draw.io illustration for part c. The exercise also involved modifying
and simulating the supplied examples.

This was coursework based on provided material, rather than an
independently designed FPGA system. The precise division between starter
code and my modifications is not fully documented in this archive.

## Original Illustration

![Original conceptual diagram for part c](3c.drawio.png)

The illustration is preserved as part of the original submission.
It should not be treated as a verified circuit diagram.

## Retrospective — What I Now Recognise

### Explaining signal and variable updates

My original answers describe some timing differences as assignments
“taking longer.” This was not a sufficiently precise explanation.

A better explanation would distinguish:
- Immediate updates to variables within a process
- Scheduled updates to signals
- Delta cycles, which do not themselves advance simulation time
- Clock-cycle delays introduced by the structure of a clocked design

Today, I would trace the values read and assigned during each process
execution rather than explain the behaviour as a general difference
in assignment speed.

### Timing units and input values

My written answers use picoseconds, while the assignment instructions
specify nanoseconds. The archived testbench input patterns also need
to be checked against the requested values.

I would now report timings directly from the relevant simulation and
check that the source, waveform and written explanation describe
the same experiment.

### Unknown values

My explanation of the initial `UUUUUUUU` output was too broad.

I would now identify which internal signals were uninitialised,
when reset took effect and how those values propagated through the
design, rather than attribute the result only to the input.

### Diagram notation and connections

The triangular inverter symbols in my drawing are missing their
output inversion bubbles. More importantly, the connections and
labels do not clearly explain the relationship between the signal
and variable assignments.

I would now distinguish intermediate variable values from signal
nodes and check every output connection against the source code.

Adding inverter bubbles alone would not resolve the diagram's
conceptual limitations.

### Process sensitivity

My explanation of part d could more clearly separate:
- The process execution at simulation startup
- Later executions triggered by sensitivity-list events
- Signal updates that no longer trigger the process after a signal
  is removed from that list

I would also preserve the different experimental versions separately,
making it easier to connect each explanation to the code that produced
the observation.

## What I Would Do Differently

If repeating the exercise, I would:

1. Keep a separate source version for each part.
2. Check the input patterns and timing units against the instructions.
3. Use a table to trace values across process executions and clock edges.
4. Save labelled waveforms alongside the corresponding source version.
5. Add automated output checks where appropriate.
6. Validate the diagram against the VHDL statement by statement.
7. Clearly separate observed behaviour from my explanation of its cause.

These are retrospective improvements, not changes applied to the
archived submission.

## Learning Value

This exercise helped introduce the distinction between writing
sequential-looking VHDL statements and understanding their simulation
semantics.

Revisiting the submission also shows the importance of precise technical
explanations: observing an output change is not the same as correctly
explaining why it changed.

## Verification Status

The files are presented as a historical learning exercise, not as a
validated reference implementation.

The simulations have not been rerun as part of this documentation update.
The original answers and illustration remain unchanged, including the
limitations discussed above.
