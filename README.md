# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)
![Screenshot 2025-05-09 113100](https://github.com/user-attachments/assets/de17cf9d-9b4e-460c-8403-be75813c340a)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)
# Testbench :
```
timescale 1ns / 1ps
module up_down_counter_tb();
reg clk;
reg reset;
reg up_down;
reg enable;
wire [3:0] count;
up_down_counter uut(
    .clk(clk),
    .reset(reset),
    .up_down(up_down),
    .enable(enable),
    .count(count)
);

initial begin
    clk = 0;
    forever #5 clk = ~clk;  
end

initial begin

    reset = 1;
    up_down = 1;
    enable = 0;
    

    #20 reset = 0;
    

    #10 enable = 1;
    #200;  
    

    up_down = 0;
    #200;  
    

    enable = 0;
    #50;
    

    reset = 1;
    #20 reset = 0;
    

    #50;
    

    $finish;
end

initial begin
    $monitor("Time: %t, Reset: %b, Up/Down: %b, Enable: %b, Count: %b", 
             $time, reset, up_down, enable, count);
end
endmodule
```
# DESIGN :
```
imescale 1ns / 1ps module up_down_counter( input wire clk,
input wire reset,
input wire up_down,
input wire enable,
output reg [3:0] count );

always @(posedge clk or posedge reset) begin
    if (reset) 
    begin
        count <= 4'b0000;
    end

    else if (enable)
    begin
        if (up_down)
        begin
            count <= count + 1'b1;
        end
        else begin
            count <= count - 1'b1;
        end
    end
end
endmodule
```
◦ SDC (Synopsis Design Constraint) File (.sdc)
## Synthesis RTL Schematic :
![Screenshot 2025-05-09 114632](https://github.com/user-attachments/assets/7e54eed0-e6a3-4c21-9211-8f7d84219746)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

•	The SDC File must contain the following commands;
```

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]
```

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.
![Screenshot 2025-05-09 115020](https://github.com/user-attachments/assets/145ddff7-6d68-4881-b4c6-5c020a38e510)

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist

#### Area report:
![Screenshot 2025-05-20 155633](https://github.com/user-attachments/assets/a73fcbef-8b31-41fa-8aa0-b8636b855a39)

#### Power Report:
![Screenshot 2025-05-20 155730](https://github.com/user-attachments/assets/fb89dda5-b23c-4edd-8981-07220130e541)

#### Timing Report: 
![Screenshot 2025-05-09 115147](https://github.com/user-attachments/assets/76238d71-4f2f-4e11-a41e-c387c018e3ba)

#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.
![Screenshot 2025-05-09 114920](https://github.com/user-attachments/assets/94db28c8-719b-47b7-a192-5c9da30aa6ed)






