# sram_iverilog

## Project Description
### Title:
**SRAM Design Using Verilog**

**Overview:**
This project focuses on designing and simulating an SRAM (Static Random Access Memory) module using Verilog, a powerful hardware description language. The simulation is carried out using Icarus Verilog (iverilog), a popular open-source Verilog simulation and synthesis tool, and GTKWave for waveform analysis.

 **Objective:** 
The primary goal is to create a functional SRAM design and thoroughly verify its operation through simulation. This project will serve as a comprehensive guide, providing detailed documentation and insights into the SRAM design process.

### Key Components:

**Verilog Design Files:**

**SRAM Module:**
Core design of the SRAM, including memory array, read/write logic, and control signals.

**Submodules:**
Any additional components such as decoders, multiplexers, and latches used within the SRAM design.

**Testbenches:**

**SRAM Testbench:**
A Verilog testbench to apply various test cases and stimuli to the SRAM module, verifying its functionality and performance under different scenarios.

**Simulation Scripts:**

Scripts to compile and run the Verilog files using Icarus Verilog (iverilog), facilitating seamless simulation and debugging processes.

## Documentation:

**Design Specifications:**
Detailed description of the SRAM design, including block diagrams, signal descriptions, and functionality.

**Simulation Results:**
Comprehensive analysis of simulation outputs, highlighting the SRAM's performance metrics and verifying the correctness of the design.

## Usage:
- first install the iverilog using the command
  ```
  sudo apt install iverilog
  ```
- to start the iverilog type the command
  ```
  iverilog -v
  ```
  ![image](https://github.com/user-attachments/assets/7f5a71f7-51e0-46e4-b21d-38d7765a6f16)


- Compile the Verilog Files:
- create the run file using any editor
 ```
  module sram #( parameter DATA_WIDTH = 8, parameter ADDR_WIDTH = 4 )( input wire clk, input wire [ADDR_WIDTH-1:0] address, input wire [DATA_WIDTH-1:0] data_in,  input wire write_enable, output reg [DATA_WIDTH-1:0] data_out );
    reg [DATA_WIDTH-1:0] memory [0:2**ADDR_WIDTH-1];
    always @(posedge clk)
    begin
        if (write_enable)
        begin
            // Write operation
            memory[address] <= data_in;
        end
        else
        begin
            // Read operation
            data_out <= memory[address];
        end
    end
endmodule
```
tesbench
```
module sram_tb;   
    parameter DATA_WIDTH = 8;
    parameter ADDR_WIDTH = 4;
    reg clk;
    reg [ADDR_WIDTH-1:0] address;
    reg [DATA_WIDTH-1:0] data_in;
    reg write_enable;
    wire [DATA_WIDTH-1:0] data_out;
    sram #(DATA_WIDTH, ADDR_WIDTH) uut (
        .clk(clk),
        .address(address),
        .data_in(data_in),
        .write_enable(write_enable),
        .data_out(data_out)
    );

    // Clock generation
    initial 
    begin
        clk = 0;
        forever #5 clk = ~clk; 
    end
    initial 
    begin
        // Dump waveforms
        $dumpfile("sram_wave.vcd");
        $dumpvars(0, sram_tb);

        // Test 1: Write 42 to address 0
        #10 write_enable = 1; 
        data_in = 8'd42; 
        address = 4'd0;
        #10 write_enable = 0;

        // Test 2: Write 99 to address 5
        #10 write_enable = 1;
        data_in = 8'd99; 
        address = 4'd5;
        #10 write_enable = 0;

        // Test 3: Read back data from address 0 and 5
        #10 address = 4'd0;  // Read address 0
        #10 address = 4'd5;  // Read address 5

        #20 $finish;
    end
endmodule
```
- can add more cases to verify more efficiently
- to run the simulation
```
iverilog -o sram sram.v sram_tb.v
vvp sram_sim
```
- Analyze the Simulation Results with GTKWave:

- Generate a VCD (Value Change Dump) file during simulation.

- Launch GTKWave to view the waveforms:

```
gtkwave sram_wave.vcd
```
Use GTKWave to analyze the waveforms and verify the performance and correctness of the SRAM design.
