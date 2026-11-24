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

## Testbench Program



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

**## Output**



## Result

Thus, the D Flip-Flop and T Flip-Flop were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.
