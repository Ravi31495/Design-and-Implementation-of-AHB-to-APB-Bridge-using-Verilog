# Verification Notes

The recovered simulation environment uses a small AHB master stimulus module, the bridge top module, and a lightweight APB-side behavioral model.

## Transactions present in the recovered testbench

| Test | Stimulus task | Purpose |
|---|---|---|
| Single write | `single_write()` | Basic AHB write converted to APB write |
| Single read | `single_read()` | Basic AHB read converted to APB read |
| Incrementing burst write | `burst_incr4_write()` | Multiple write transfers with incrementing addresses |
| Incrementing burst read | `burst_incr4_read()` | Multiple read transfers with incrementing addresses |

The project report documents waveform evidence for single read/write and burst read/write operations, plus an RTL synthesis view.

## Simulation model limitation

`sim/apb_slave_model.v` is a simple behavioral model. During an APB read in the ENABLE phase it produces a pseudo-random value. It is useful for exercising the bridge data path, but it is not a complete APB peripheral or protocol checker.

## Reproduction

Use the recovered ModelSim project configuration when available, or compile the Verilog files manually in ModelSim. The default call in `sim/top_tb.v` runs the recovered incrementing burst-read stimulus; the other tasks can be enabled one at a time.

## Recovery caveat

Generated ModelSim waveform databases and Quartus compilation databases are not part of the source repository. The useful reproducible material is the RTL, testbench, project documentation and transaction evidence.
