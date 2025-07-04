# Day 1: Kickstarting Verilog RTL Design & Logic Synthesis

Welcome to your journey into the fascinating world of digital hardware design!

On Day 1, you’ll begin by learning the basics of **Verilog**, a hardware description language used to design digital circuits. You’ll also dive into practical tools like **Icarus Verilog** (for simulation) and **Yosys** (for synthesis). By the end of today, you will have written, simulated, and synthesized your first Verilog-based digital circuit!

---

## Agenda for Day 1

1. Simulator, Design & Testbench 
2. Setting Up with Icarus Verilog
3. Practical: Simulating Your Design with Icarus Verilog
4. Introduction to Yosys – Verilog Synthesis Tool
5. Practical: Synthesizing a Design Using Yosys

---

## 1. Simulator, Design & Testbench 

### Simulator

A simulator mimics the behavior of a digital circuit on your computer. It helps verify that your design performs as expected *before* it's built or implemented on hardware.

### Design

This is your Verilog code – a description of what your circuit is supposed to do. Think of it as your digital blueprint.

### Testbench

A testbench is like an automated tester. It provides various input signals to your design and observes the outputs, ensuring everything behaves as it should under different conditions.

---

## 2. Setting Up with Icarus Verilog

**Icarus Verilog** is an open-source tool used for compiling and simulating Verilog designs. Here's the typical workflow:

1. Write your **design** and **testbench** files in Verilog.
2. Use `iverilog` to compile both files into a single executable.
3. Run the executable to generate a **VCD file** (Value Change Dump) that records how signals change over time.
4. Use **GTKWave** to view this waveform file and visually inspect your design’s behavior.

---

## 3. Practical: Simulating Your Design with Icarus Verilog

### Example Design: `good_mux.v`

### Example Testbench: `tb_good_mux.v`

### Step-by-Step

**Step 1: Compile the files**

```bash
iverilog good_mux.v tb_good_mux.v
```

* This creates an executable file: `a.out`

**Step 2: Run the simulation**

```bash
./a.out
```

* This will generate a VCD file (e.g., `tb_good_mux.vcd`)

**Step 3: View the waveform**

```bash
gtkwave tb_good_mux.vcd
```

* Analyze how your signals change over time, and verify input/output behavior.

---

## 4. Introduction to Yosys – Verilog Synthesis Tool

### What is Yosys?

Yosys is an open-source tool that takes your Verilog RTL design and converts it into a **gate-level representation**. This process is called **logic synthesis** – translating high-level logic into gates like AND, OR, NOT, etc., that can be physically implemented.

Yosys is widely used in open-source silicon initiatives, academia, and personal learning environments for RTL design and synthesis.

---

## 5. Practical: Synthesizing a Design Using Yosys

Let’s run a synthesis example using the `good_mux.v` design.

### Yosys Command Sequence

```bash
yosys
```

Once in the yosys shell, run the following steps:

1. **Read the standard cell library**

```tcl
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

2. **Import the Verilog design**

```tcl
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
```

3. **Run synthesis**

```tcl
synth -top good_mux
```

4. **Map the design to standard cells**

```tcl
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

5. **Visualize the synthesized netlist**

```tcl
show
```

---

## From RTL to Gate-Level

* **RTL (Register Transfer Level):** A design abstraction that describes behavior using registers and logic operations.
* **Synthesis:** The process of converting RTL into a network of logic gates.
* **Netlist:** The result of synthesis – a description of all gates and how they’re connected.

---

## What Is a `.lib` File?

A `.lib` (Liberty format) file defines the characteristics of standard cells in a particular technology node (e.g., 130nm). It contains:

* Logical functions of each gate (e.g., AND, OR, NOT).
* Timing and power info.
* Variants of the same logic gate optimized for different performance/power tradeoffs.

---

### Fast vs Slow Cells – Why Do They Exist?

Cells in a standard cell library may have different drive strengths or delays:

* **Fast cells**: Consume more power, switch quickly.
* **Slow cells**: Use less power, switch slowly.

### How are cells selected?

The synthesis tool picks the appropriate version of a gate based on performance, area, and power constraints defined by the designer or the tool settings.

---
