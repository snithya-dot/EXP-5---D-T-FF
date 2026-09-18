# EXP-5-DFF-TFF
# Design and Verification of D Flip-Flop and T Flip-Flop
## Aim
To design and verify a D Flip-Flop and a T Flip-Flop using Verilog HDL, and to observe their behaviour using a testbench and VCD waveform.
## Theory / Synopsis
A D Flip-Flop (Data / Delay Flip-Flop) is an edge-triggered sequential element that transfers the value present at its input D to the output Q on the active (rising) edge of the clock. It is commonly used to store one bit of data and eliminates the race-around problem seen in level-triggered latches.
A T Flip-Flop (Toggle Flip-Flop) is an edge-triggered sequential element whose output toggles (complements) on every active clock edge when the toggle input T is high, and holds its previous state when T is low. It is widely used in counter and frequency-divider circuits.
Both flip-flops used here are also provided with an active-high asynchronous reset that forces Q to 0 irrespective of the clock.
## Boolean / Characteristic Equations
### D Flip-Flop
Q(next) = D
### T Flip-Flop
Q(next) = T ⊕ Q   (Q(next) = Q if T = 0, Q(next) = Q' if T = 1)
## Files to be Created
<img width="625" height="50" alt="image" src="https://github.com/user-attachments/assets/65034a51-5642-46ac-940a-f7ee975cea9c" />

## Design / RTL Program
// gedit dff_tff.v

 ```
module dff (
    input  wire clk,   // clock
    input  wire rst,   // asynchronous active-high reset
    input  wire D,     // data input
    output reg  Q      // output
);
    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 1'b0;
        else
            Q <= D;
    end
endmodule
 
module tff (
    input  wire clk,   // clock
    input  wire rst,   // asynchronous active-high reset
    input  wire T,     // toggle input
    output reg  Q      // output
);
    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 1'b0;
        else if (T)
            Q <= ~Q;
        else
            Q <= Q;
    end
endmodule

```
## Testbench Program


// gedit tb5.v


 module tb5;
 
    reg  clk, rst, D, T;
    wire Qd, Qt;
 
    // Instantiate the D Flip-Flop
    dff dff_uut (
        .clk(clk),
        .rst(rst),
        .D(D),
        .Q(Qd)
    );
 
    // Instantiate the T Flip-Flop
    tff tff_uut (
        .clk(clk),
        .rst(rst),
        .T(T),
        .Q(Qt)
    );
 
    // ---- Clock generation: 10 time-unit period ----
    initial clk = 0;
    always #5 clk = ~clk;
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("dff_tff.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb5);            // dump all signals in this testbench hierarchy
 
        $monitor("Time=%0t rst=%b clk=%b | D=%b Qd=%b | T=%b Qt=%b",
                  $time, rst, clk, D, Qd, T, Qt);
 
        // ---- Apply asynchronous reset ----
        rst = 1; D = 0; T = 0;
        #7  rst = 0;
         // ---- Apply test vectors on successive clock edges ----
        @(negedge clk); D = 1; T = 1;
        @(negedge clk); D = 0; T = 1;
        @(negedge clk); D = 1; T = 0;
        @(negedge clk); D = 0; T = 0;
        @(negedge clk); D = 1; T = 1;
        @(negedge clk); D = 1; T = 1;
 
        // ---- Apply reset again mid-simulation ----
        @(negedge clk); rst = 1;
        @(negedge clk); rst = 0; D = 1; T = 1;
 
        #20 $finish;
    end
    endmodule

## Truth Table

<img width="349" height="264" alt="image" src="https://github.com/user-attachments/assets/d05e6f9d-ff68-466e-9eb8-52e22fa1b961" />

## Simulation Procedure

STEP 1 – Open Terminal

Open a terminal in the experiment folder.

STEP 2 – Load Synopsys Environment
source /synopsys/start.sh

STEP 3 – Compile Using VCS
vcs dff_tff.v tb.v -full64
If compilation is successful, VCS generates the simulation executable:
simv

STEP 4 – Run Simulation
./simv
The terminal displays the reset, clock, D/T inputs and the corresponding outputs Qd and Qt at every change (via $monitor). A VCD waveform file is also generated:
dff_tff.vcd

STEP 5 – Open DVE
dve -full64
Other option:
dve -full64 &
A DVE environment will open.

## DVE Waveform Verification

In DVE:
●	Open the testbench hierarchy.

●	Locate the signals: clk, rst, D, Qd, T, Qt.

●	Add the signals to the waveform window.

●	Run / inspect the waveform.

●	Verify that Qd follows D on every rising edge of clk when rst = 0.

●	Verify that Qt toggles on every rising edge of clk when T = 1 and rst = 0, and holds its value when T = 0.

●	Confirm that both Qd and Qt go to 0 immediately whenever rst = 1, independent of clk.

The waveform should agree with the truth table.

## Expected Result

The D Flip-Flop and T Flip-Flop were realized using Verilog HDL:

Q(next) = D   (D Flip-Flop)
Q(next) = T ⊕ Q   (T Flip-Flop)

The design was compiled and simulated using Synopsys VCS, and the functionality was verified using DVE waveform analysis, matching the expected truth table.

## Output
<img width="595" height="369" alt="image" src="https://github.com/user-attachments/assets/38290e4d-a813-4e0c-b891-68044c90623b" />

## Viva-Voce Questions

●	What is the difference between a latch and a flip-flop?

●	Why is the D flip-flop also called a 'delay' element?

●	How is a T flip-flop derived from a JK flip-flop?

●	What is the characteristic equation of a T flip-flop, and what does it mean when T = 1?

●	What is the difference between synchronous and asynchronous reset? Which one is used here?

●	Why is posedge used in the sensitivity list, and what does 'posedge clk or posedge rst' achieve?

●	What is the purpose of a Verilog testbench?

●	Why is a VCD file generated, and what does $dumpvars(0, tb) do?

●	How would you construct a T flip-flop using a D flip-flop and an XOR gate?

●	How can a chain of T flip-flops be used to build an asynchronous binary counter?

## Result

Thus, the D Flip-Flop and T Flip-Flop were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.
