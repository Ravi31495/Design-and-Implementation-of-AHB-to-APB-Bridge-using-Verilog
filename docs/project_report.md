# AHB to APB Bridge Design using Verilog HDL

## Abstract

This project presents an AHB-to-APB bridge implemented in Verilog HDL. The bridge provides an interface between the high-speed, pipelined Advanced High-performance Bus (AHB) and the low-power Advanced Peripheral Bus (APB). It acts as an AHB slave and APB master, translating valid AHB transfers into the APB SETUP and ENABLE sequence.

The project report describes an AHB slave interface, address decoding and an APB controller. The RTL was functionally exercised in ModelSim and the design was synthesized using Intel Quartus Prime.

## Objectives

1. Study the AMBA AHB and APB bus protocols and their signal-level operation.
2. Capture AHB transactions directed to the APB address space.
3. Decode addresses and generate the corresponding APB peripheral select.
4. Implement the APB SETUP/ENABLE control sequence with an FSM.
5. Integrate the bridge into a top-level RTL module.
6. Verify read/write behavior using ModelSim testbenches.
7. Synthesize the RTL with Intel Quartus Prime and inspect the resulting design.

## Architecture

The recovered design is organized around these functions:

- **AHB Slave Interface:** captures/pipelines address, write data and write direction, detects valid transfers and decodes the APB address window.
- **APB Controller:** controls the bridge state machine and generates `PADDR`, `PWDATA`, `PWRITE`, `PSEL` and `PENABLE`.
- **Bridge Top:** integrates the two blocks.
- **Simulation models:** an AHB master stimulus and a lightweight APB-side model exercise the bridge.

## Protocol flow

For a valid AHB transfer, the bridge captures the request and sequences the APB side through SETUP and ENABLE. For writes, AHB write data is transferred to `PWDATA`. For reads, APB `PRDATA` is returned through the bridge toward `HRDATA`.

## Address decoding

The recovered decoder uses three APB select windows:

| Address range | Select |
|---|---|
| `0x8000_0000` to `0x83FF_FFFF` | `PSEL[0]` |
| `0x8400_0000` to `0x87FF_FFFF` | `PSEL[1]` |
| `0x8800_0000` to `0x8BFF_FFFF` | `PSEL[2]` |

## Verification

The source archive contains stimulus tasks for single read, single write, incrementing burst read and incrementing burst write operations. The accompanying project report describes waveform results for these cases and an RTL synthesis view.

## Tools and target

- Verilog HDL
- ModelSim
- Intel Quartus Prime
- Cyclone V
- Device: `5CSXFC6D6F31I7ES`

## Applications

The report identifies use cases such as connecting high-performance processor/DMA-side components to low-power UART, timer, GPIO and interrupt-controller peripherals, as well as using the design as an educational AMBA bridge reference.

## Source status

This markdown document is a repository-friendly reconstruction of the attached project report. The original DOCX report is not reproduced verbatim here. The repository source is based on the recovered project archive supplied with this project.
