# APB UVM Testbench (No DUT)

This repository contains a UVM (Universal Verification Methodology) testbench developed for the AMBA APB protocol.  
It is designed as a reusable environment to generate, drive, monitor, and check APB transactions without an attached DUT (Device Under Test).  
The testbench focuses on building a complete UVM environment to demonstrate sequence-driven verification, monitoring, and scoreboarding.

---

## Repository Structure

apb_if.svh  
Defines the APB SystemVerilog interface (signals, clocking blocks, modports for driver and monitor).

apb_rw.svh  
Transaction class (apb_rw) defining APB read/write operations.

apb_sequences.svh  
Sequence classes (for example, apb_base_seq) to generate randomized read/write transactions.

apb_driver_seq_mon.svh  
Driver (drives APB interface), Monitor (captures APB transactions), and related components.

apb_scoreboard.svh  
Reference model using a memory map; checks read/write transactions for correctness.

apb_agent_env_config.svh  
UVM agent, environment, and configuration objects. Connects all components together.

apb_test.svh  
Test classes to start sequences and run the environment (apb_base_test).

testbench.sv  
Top-level testbench module that instantiates the APB interface and starts the UVM test.

---

## How It Works

1. Sequences generate random APB read/write transactions.  
2. Driver drives transactions onto the APB bus.  
3. Monitor observes transactions on the interface and sends them to the scoreboard.  
4. Scoreboard stores expected write values and checks read values against reference memory.  
5. The environment runs without a DUT, focusing on validating the UVM flow.

---

## Running the Test

Compile and run with any SystemVerilog simulator supporting UVM (for example, VCS, Questa, Xcelium):

vcs -sverilog -ntb_opts uvm-1.2 \
    testbench.sv \
    apb_if.svh apb_rw.svh apb_sequences.svh \
    apb_driver_seq_mon.svh apb_scoreboard.svh \
    apb_agent_env_config.svh apb_test.svh \
    +incdir+.
./simv +UVM_TESTNAME=apb_base_test

## Features

- UVM-compliant agent, monitor, driver, sequencer, and scoreboard  
- Randomized read/write sequences  
- Scoreboard with reference memory model  
- Protocol violation checks (for example, missing ENABLE after SETUP)

---

## Next Steps

- Add an APB DUT (for example, APB RAM) to integrate and verify actual functionality  
- Extend sequences to cover corner cases and error conditions  
- Add functional coverage for better verification completeness

---

