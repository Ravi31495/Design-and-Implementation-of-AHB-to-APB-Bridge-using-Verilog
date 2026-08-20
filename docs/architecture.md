# Architecture

The project report describes the bridge as three cooperating functional areas: an AHB slave interface, address decoding, and an APB controller. In the recovered RTL, address decoding is implemented inside the AHB slave interface and the APB controller contains the transaction state machine.

## Transaction flow

```text
AHB Master
   │ HADDR / HTRANS / HWRITE / HWDATA
   ▼
AHB Slave Interface
   │ latched address / write data / direction / valid
   ▼
APB Controller FSM
   │ PADDR / PWRITE / PWDATA / PSEL / PENABLE
   ▼
APB Peripheral
   │ PRDATA
   └──────────────► bridge / AHB read-data path
```

## APB sequencing

The documented conceptual sequence is:

```text
IDLE
  │ valid AHB transfer
  ▼
SETUP
  │ next clock
  ▼
ENABLE
  │ transfer complete
  └──────────────► IDLE
```

For a read, APB `PRDATA` is returned toward the AHB read-data path. For a write, the captured AHB write data is driven as APB `PWDATA`.

## Address map

The recovered decoder divides the APB address space into three windows:

```text
0x8000_0000 ─────────────── 0x83FF_FFFF   PSEL[0]
0x8400_0000 ─────────────── 0x87FF_FFFF   PSEL[1]
0x8800_0000 ─────────────── 0x8BFF_FFFF   PSEL[2]
```

## Main RTL blocks

- `ahb_slave_interface` — captures/pipelines AHB-side address, data and write control, determines valid transfers and generates the peripheral select code.
- `apb_controller` — implements the recovered bridge FSM and registers the APB-side outputs.
- `bridge_top` — connects the AHB slave interface and APB controller.
