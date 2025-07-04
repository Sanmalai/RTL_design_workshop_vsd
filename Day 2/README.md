# Day 2: Timing Libraries, Synthesis Strategies, and Flip-Flop Coding Styles

## 1. Timing Libraries and the Sky130 PDK

### What is the Sky130 PDK?

The Sky130 Process Design Kit (PDK) is an open-source 130nm CMOS technology developed by SkyWater Technology in collaboration with Google. It includes the models, libraries, and design rules necessary for digital and analog IC design.

### About `tt_25c_1v80.lib`

This is a Liberty-format `.lib` file representing standard cell behavior at:

* Process: Typical-Typical (TT)
* Temperature: 25°C
* Voltage: 1.80V

It is used in:

* Logic synthesis (e.g., with Yosys)
* Static timing analysis (e.g., OpenSTA)
* Physical design tools (e.g., OpenROAD)

### What’s Inside a `.lib` File?

* Logic functionality of each standard cell
* Propagation delay and transition time
* Setup and hold timing data
* Power consumption (dynamic and leakage)
* Multiple drive strength versions of the same logic cell

---

## 2. Hierarchical vs Flat Synthesis

Synthesis translates RTL code into a gate-level netlist using technology-specific cells. This can be done using either a flat or hierarchical approach.

### Flat Synthesis

* All modules are flattened into one level before optimization
* Allows full visibility across module boundaries
* Enables global optimizations such as:

  * Logic restructuring
  * Area/timing/power trade-offs

**Advantages:**

* Better optimization
* Ideal for small to mid-sized designs

**Disadvantages:**

* Requires more memory and time
* Harder to debug due to loss of modular structure

### Hierarchical Synthesis

* Preserves module boundaries during synthesis
* Useful for large designs or reusable IPs
* Modules are synthesized independently and connected at the top level

**Advantages:**

* Faster and more memory-efficient
* Easier to manage and reuse code

**Disadvantages:**

* Less cross-boundary optimization
* May result in suboptimal timing or area

### Summary Table

| Use Case                        | Recommended Approach   |
| ------------------------------- | ---------------------- |
| Small to mid-size design        | Flat synthesis         |
| SoCs with multiple IPs          | Hierarchical synthesis |
| Emphasis on timing optimization | Flat synthesis         |
| Emphasis on modular reuse       | Hierarchical synthesis |

---

## Module-level vs Submodule-level Synthesis

### Submodule-level Synthesis

* Individual modules are synthesized independently
* Reused across multiple instances
* Good for IPs and replicated logic blocks

### Module-level Synthesis

* Synthesis is done at a higher level (e.g., top module)
* Allows context-based optimization of submodules
* Useful for performance-critical logic

| Scenario                             | Preferred Method          |
| ------------------------------------ | ------------------------- |
| Reused logic/IPs                     | Submodule-level synthesis |
| Unique, performance-sensitive blocks | Module-level synthesis    |

---

## 3. Flip-Flop Coding Styles

### Asynchronous Reset D Flip-Flop

```verilog
module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @ (posedge clk or posedge async_reset)
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

### Asynchronous Set D Flip-Flop

```verilog
module dff_async_set (input clk, input async_set, input d, output reg q);
  always @ (posedge clk or posedge async_set)
    if (async_set)
      q <= 1'b1;
    else
      q <= d;
endmodule
```

### Synchronous Reset D Flip-Flop

```verilog
module dff_syncres (input clk, input sync_reset, input d, output reg q);
  always @ (posedge clk)
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

---

## 4. Simulation and Synthesis Flow (with Icarus Verilog and GTKWave)

1. Compile the design and testbench:

   ```bash
   iverilog dff_asyncres.v tb_dff_asyncres.v
   ```

2. Run the simulation:

   ```bash
   ./a.out
   ```

3. Open waveform viewer:

   ```bash
   gtkwave tb_dff_asyncres.vcd
   ```
