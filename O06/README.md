# Assignment 6 — Modular Seven-Segment Display Control

A learning exercise completed during my degree, exploring modular
VHDL design through a controller for a two-digit seven-segment display.

The assignment brought together decoding functions, counter-based
timing, display multiplexing, a ROM-based message sequence and
structural component integration.

> **Archived coursework**
>
> The original implementation and supporting files are preserved.
> This README adds context and retrospective reflection; it does not
> present the work as a corrected or newly validated design.

## Assignment Overview

The exercise progressed through several stages:

| Part | Focus |
| --- | --- |
| a | Implement a binary-to-seven-segment decoding function |
| b | Control two digits through time multiplexing |
| c | Generate a repeating message using a self-test unit and ROM |
| d | Integrate the modules and implement the system on a ZedBoard |
| e | Optionally investigate button counting and contact bounce |

The hardware implementation stage required simulation, pin constraints,
synthesis and implementation reports, and a programming bitstream.

These were assignment requirements, not a claim that every deliverable
has been independently verified in this archive.

## Original Documentation

- [Assignment overview and delivery checklist](taskdescription.md)
- [Detailed tasks](task.md)
- [Design methodology and theory](theory.md)

The original instructions and reference material are retained separately
from this retrospective.

## Repository Contents

| Path | Description |
| --- | --- |
| [`src/`](src/) | Archived VHDL implementation |
| [`test/`](test/) | Archived testing material |
| [`images/`](images/) | Illustrations accompanying the documentation |
| [`taskdescription.md`](taskdescription.md) | Original assignment overview |
| [`task.md`](task.md) | Detailed exercise instructions |
| [`theory.md`](theory.md) | Course-provided design guidance |
| [`pmodssd_rm.pdf`](pmodssd_rm.pdf) | Seven-segment display reference manual |
| [`ZedBoard_HW_UG_v2_2.pdf`](ZedBoard_HW_UG_v2_2.pdf) | ZedBoard hardware reference |
| [`ZedBoard_HW_UG_v1_1.pdf`](ZedBoard_HW_UG_v1_1.pdf) | Reference for older ZedBoards |

## Implementation Structure

### Seven-Segment Decoder Package

`src/seg7_pkg.vhd` contains two decoding functions:

- `bin2ssd` maps four-bit values to hexadecimal display patterns.
- `bin2ssd_alt` implements the alternative character encoding used
  for the message sequence.

Keeping the lookup functions in a package allows them to be reused
by the display controllers.

### Multiplexed Display Controller

`src/seg7ctrl.vhd` uses a counter to alternate between the two digits.

The selection signal `c` determines whether `d0` or `d1` is decoded
onto the shared segment outputs.

`src/seg7ctrl_arch.vhd` contains a related controller using the
alternative character encoding. Despite its filename, it declares
a separate entity named `seg7ctrl_arch`, rather than only an
alternative architecture of `seg7ctrl`.

### ROM-Based Message Generator

`src/self_test_unit.vhd` stores a sequence of paired character codes
in a constant ROM.

A counter controls when the next pair is sent to `d0` and `d1`.
The sequence wraps around after the final entry.

The intended hardware configuration advances the message at roughly
one-second intervals, while the display controller refreshes the
individual digits independently.

### Structural Integration

`src/self_test_system.vhd` connects the message generator to the
alternative display controller through two internal four-bit signals.

The top-level interface provides clock and reset inputs, a digit-select
output and the seven segment-control outputs.

## Hardware Demonstration: LED Message

This assignment successfully bridges the gap between code and physical hardware by implementing a text display on the board. Check out the demonstration video below, which shows the LEDs successfully outputting the programmed message in real-time.

[🎬 Watch the LED Message Demonstration](./message.mp4)

## Retrospective — What I Would Approach Differently

### Reset Every State Element Deliberately

The standard display controller resets both its counter and the
digit-select signal `c`.

The alternative controller resets only the counter, even though
it later updates `c` using `not c`.

This leaves the starting digit selection undefined in ordinary
VHDL simulation. Today, I would explicitly reset all state needed
for deterministic startup and test reset behaviour in each module.

### Make Timing Definitions Precise

The counters compare their current value with a limit before
returning to zero. Starting from zero, this produces an interval
of the limit plus one clock cycles.

I would now define whether a parameter represents a terminal count
or a number of cycles, then use that definition consistently in
the implementation, comments and tests.

I would also distinguish the digit-switching interval from the
refresh period of an individual digit.

### Make Generic Parameters Consistent

The self-test unit declares generic ROM dimensions but also uses
fixed values such as `15`, `7 downto 4` and `3 downto 0`.

Those assumptions fit the original 16-entry, eight-bit message
table but limit how reusable the parameters really are.

Today, I would either keep those dimensions explicitly fixed or
derive the addressing and slicing consistently from the intended
configuration.

### Clarify Message Ordering and Startup

The ROM uses descending indices, while playback accesses it through
`15-next_addr`. Reset selects `ROM(0)`, which differs from the first
entry selected during normal playback.

I would now document the intended first displayed pair, the reset
display and the complete wraparound sequence.

A test checking every message entry would make the ordering easier
to verify than relying only on visual inspection.

### Reduce Duplication Between Controllers

The standard and alternative controllers duplicate much of the
counter and digit-selection logic.

I would now consider sharing that control structure and separating
it from the character encoding. This would reduce the risk of a
reset or timing fix being applied to one version but not the other.

I would also make entity and architecture names match their roles
more clearly.

### Separate Simulation Configuration from Hardware Configuration

The assignment permits shorter counter intervals during simulation.

If repeating the exercise, I would make those overrides explicit
in the simulation configuration and document the hardware values
separately.

I would also expose the relevant parameters through the top-level
module so that fast simulation would not require editing the
archived design.

## Verification I Would Add Today

I would use automated checks for:
- Every decoder entry
- The relationship between `c`, the selected input and the segment output
- Exact counter intervals
- Reset asserted between clock edges
- Deterministic startup of both controller variants
- The complete message sequence and wraparound
- Integration between the message generator and display controller

Physical testing would remain a separate step for checking readability,
flicker, brightness and the actual board connections.

## Learning Value

This exercise connected several earlier topics in a larger design:
- Reusable functions and packages
- Combinational decoding
- Counters and clocked state
- Time-multiplexed outputs
- ROM-based sequencing
- Structural module integration
- Hardware documentation and implementation constraints

Looking back, the main lesson is that module integration involves more
than connecting ports. Reset behaviour, timing assumptions, parameter
definitions and startup sequences must also agree.

## Attribution and Archive Status

This work was completed from course-provided instructions and reference
material. The implementation is preserved as my coursework, while the
original documentation and hardware manuals retain their respective
authorship.

The precise boundary between supplied scaffolding and my modifications
is not fully documented here.

The simulations have not been rerun, and physical board operation has
not been reverified as part of this documentation update. This README
does not claim lab approval, timing closure or completion of the
optional button-counting exercise.

The retrospective describes changes I would consider today; it does
not indicate that those changes have been applied.
