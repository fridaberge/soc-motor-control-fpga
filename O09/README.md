# Assignment 9 — Pipelined RGB-to-Grayscale Conversion

Coursework exploring arithmetic bit widths, overflow handling
and pipelining through an RGB-to-grayscale converter.

The exercise focused on dividing arithmetic into pipeline stages
while keeping pixel data and control signals aligned.

## Assignment Overview

The assignment involved:
- Answering questions about bit growth in arithmetic expressions.
- Drawing the original grayscale converter's datapath.
- Designing a two-stage pipelined version.
- Accounting for the registers used in both designs.
- Updating the testbench for the additional latency.
- Converting an image and inspecting the grayscale output.

Synthesis and timing analysis were optional extensions.

## Conversion Approach

The converter multiplies each colour channel by an integer weight,
adds the products and selects the scaled result.

For the default eight-bit configuration, the supplied weights are:
- Red: 76
- Green: 150
- Blue: 29

If the weighted sum exceeds the supported range, the output saturates
to its maximum value rather than wrapping around.

## Pipeline Structure

The archived VHDL separates the calculation into two stages:

1. Multiply the colour channels by their weights and register
   the three products.
2. Add the registered products, detect overflow and register
   the grayscale result.

The input-valid signal is delayed alongside the calculation so that
`Y_valid` corresponds to the output pixel.

The pipeline adds latency while allowing successive pixels to
occupy different stages.

### Two-Stage Pipeline Datapath

![Datapath of the two-stage RGB-to-grayscale pipeline](figure.png)

The first stage registers the three weighted colour products and
delays the input-valid signal. The second stage adds the products,
detects overflow and registers the grayscale output and control signals.

**Diagram correction:** The labels `r_G` and `r_B` directly below
`f_R` should read `f_G` and `f_B`. These refer to the multiplication
results before the first-stage registers, not the registered products.

For the default eight-bit configuration:
- Stage 1 contains 48 product bits and one valid bit: 49 flip-flops.
- Stage 2 contains eight output bits, one overflow bit and one valid
  bit: 10 flip-flops.
- The two-stage design therefore contains 59 flip-flops.

## Repository Contents

| Path | Description |
| --- | --- |
| [`src/grayscale.vhd`](src/grayscale.vhd) | Pipelined converter |
| [`test/tb_grayscale.py`](test/tb_grayscale.py) | Image stimulus, checks and output generation |
| [`ass9.drawio.pdf`](ass9.drawio.pdf) | Archived diagram PDF |
| [`diagrams.png`](diagrams.png) | Archived diagram image |
| `grayscale(4).drawio` | Editable diagram source |
| [`DSP48.png`](DSP48.png) | DSP illustration accompanying the exercise |
| [`taskdescription.md`](taskdescription.md) | Original instructions and supplied starting code |

## Testing Approach

The testbench uses cocotb and Pillow to:
- Read RGB pixels from an image.
- Drive the converter with pixel data.
- Monitor grayscale values, validity and overflow.
- Build and save a grayscale image.

The archived configuration reads `images/lion.jpg`, samples every
fourth pixel in each direction and saves `output/grayscale.png`.

The validity and grayscale checks include additional clock waits
to accommodate the pipeline.

The presence of these checks does not establish that the archived
simulation passed.

## What I Would Do Today

- Define and test the exact input-to-output latency.
- Reset the intermediate valid register explicitly.
- Check every valid pixel using a queued reference model.
- Add directed tests for saturation, changing weights and validity gaps.
- Parameterize image paths and scaling for reproducible runs.

## Learning Value

The exercise connected:
- Arithmetic bit growth and explicit sizing.
- Fixed-point weighting and scaling.
- Saturation instead of arithmetic wraparound.
- Pipeline latency and throughput.
- Alignment of data and control signals.
- Numerical checks and visual inspection of image output.

The central lesson was that inserting registers changes more than
the datapath: the testbench and control timing must change with it.

## Attribution and Verification Status

The assignment supplied the original converter and image-based
testbench as starting material.

This directory preserves coursework modifications alongside those
course resources; it does not claim that all code was written
from scratch.

The source image's authorship is not established by this overview.

The simulation has not been rerun, and the diagrams, output image
and synthesis results have not been independently verified as part
of this documentation update.
