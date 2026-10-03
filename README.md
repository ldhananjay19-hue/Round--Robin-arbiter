4-Requester Round-Robin Arbiter

📌 Project Overview

This project implements a 4-requester Round-Robin Arbiter using synthesizable Verilog-2001.

The arbiter controls access to a shared resource among four independent requesters. It grants access to at most one requester per clock cycle while ensuring fair access and preventing starvation.

The design uses a round-robin priority mechanism that dynamically updates the priority after every successful grant.

---

🎯 Objective

Design a synthesizable 4-requester round-robin arbiter that:

- Controls access to a shared resource.
- Grants access to at most one requester per clock cycle.
- Ensures fair access among requesters.
- Prevents starvation.
- Supports dynamic request patterns.
- Correctly handles priority wrap-around.
- Provides deterministic asynchronous reset behavior.

---

🧩 Module Interface

module rr_arbiter (
    input        clk,
    input        rst_n,
    input  [3:0] req,
    output [3:0] grant,
    output       grant_valid
);

Input Signals

Signal| Width| Description
"clk"| 1 bit| Rising-edge clock
"rst_n"| 1 bit| Active-low asynchronous reset
"req"| 4 bits| Request signals from four independent requesters

Output Signals

Signal| Width| Description
"grant"| 4 bits| One-hot grant signal or "0000"
"grant_valid"| 1 bit| Indicates whether a valid grant has been issued

---

🔄 Round-Robin Arbitration

The arbiter maintains an internal priority pointer.

Starting from the current priority, the arbiter searches for an active request in circular order.

For example:

Priority = 2

Search order:
2 → 3 → 0 → 1

The first active requester found in this order receives the grant.

---

⭐ Priority Update

After a successful grant, the priority moves to the requester immediately following the granted requester.

Granted Requester| Next Priority
Requester 0| 1
Requester 1| 2
Requester 2| 3
Requester 3| 0

If there is no valid request, the priority remains unchanged.

---

🔁 Example: All Requesters Active

When:

req = 4'b1111

The grants rotate fairly:

grant = 0001
grant = 0010
grant = 0100
grant = 1000
grant = 0001
...

This ensures that no requester is granted twice before the other active requesters receive access.

---

🔀 Dynamic Requests

Requests can change every clock cycle.

The arbiter:

1. Checks the current request pattern.
2. Starts searching from the current priority.
3. Skips inactive requesters.
4. Grants the first active requester.
5. Updates priority only after a successful grant.

Example

Priority = 3
req      = 4'b0011

Search order:

3 → 0 → 1 → 2

Requester 3 is inactive, so the arbiter continues searching.

Requester 0 is active:

grant = 4'b0001

Therefore, requester 0 receives the grant.

---

🔄 Reset Behavior

The design uses an active-low asynchronous reset.

When:

rst_n = 0

the outputs must be:

grant       = 4'b0000
grant_valid = 1'b0

The initial arbitration priority starts from requester 0.

---

🛡️ Fairness and Starvation Prevention

The round-robin mechanism ensures fair access to the shared resource.

For continuously asserted requests:

req = 4'b1111

access rotates among all four requesters.

The arbiter does not repeatedly grant the same requester while other active requesters are waiting.

---

🧪 Verification

A self-checking Verilog testbench is included to verify the design.

The testbench covers:

- Reset behavior
- No requests
- Single request
- Multiple simultaneous requests
- All four requests continuously asserted
- Changing request patterns
- Request withdrawal
- Priority wrap-around
- Persistent requests
- Fairness
- Starvation prevention
- One-hot grant checking
- Grant validity checking
- Inactive requester checking
- Priority update checking
- Round-robin ordering

The testbench reports an error if:

- An inactive requester is granted.
- More than one grant is asserted.
- Round-robin ordering is violated.
- Priority changes without a valid grant.
- Reset behavior is incorrect.

---

⚙️ Design Constraints

The implementation follows the required constraints:

- Pure Verilog-2001
- Synthesizable RTL
- No SystemVerilog constructs
- No "#" delays in RTL
- No "initial" blocks for functional behavior
- No derived clocks
- No gated clocks
- No inferred latches
- Deterministic reset behavior
- At most one requester granted per cycle

---

📁 Project Structure

4-requester-round-robin-arbiter/
│
├── rtl/
│   └── rr_arbiter.v
│
├── tb/
│   └── rr_arbiter_tb.v
│
├── simulation/
│   └── waveform/
│
├── README.md
│
└── screenshots/
    └── simulation_results.png

---

▶️ Simulation

The design can be simulated using tools such as:

- AMD Vivado
- Xilinx XSim
- ModelSim
- QuestaSim
- Icarus Verilog

Vivado

1. Create a new RTL project.
2. Add "rr_arbiter.v" as a design source.
3. Add "rr_arbiter_tb.v" as a simulation source.
4. Set the testbench as the simulation top.
5. Run Behavioral Simulation.
6. Observe "req", "grant", "grant_valid", "clk", and "rst_n" in the waveform.

---

📊 Expected Behavior

Priority| Request| Expected Grant
0| "0000"| "0000"
0| "0001"| "0001"
0| "0010"| "0010"
0| "0100"| "0100"
0| "1000"| "1000"
0| "1111"| "0001"
1| "1111"| "0010"
2| "1111"| "0100"
3| "1111"| "1000"
3| "0011"| "0001"

---

💡 Key Features

- ✅ 4 independent requesters
- ✅ One-hot grant output
- ✅ Round-robin arbitration
- ✅ Fair resource sharing
- ✅ Starvation prevention
- ✅ Dynamic request handling
- ✅ Priority wrap-around
- ✅ Asynchronous active-low reset
- ✅ Synthesizable Verilog-2001
- ✅ Self-checking testbench
- ✅ Comprehensive verification

---

🚀 Future Scope

The design can be extended to support:

- Parameterized number of requesters
- Configurable arbitration policies
- Weighted round-robin arbitration
- Priority-based arbitration
- AXI/AHB-style bus arbitration
- Integration into larger SoC/interconnect designs

---

👥 Team

Silicon Sprint – Semiconductor Hackathon

Project: 4-Requester Round-Robin Arbiter

---

📜 License

This project is developed for educational and hackathon purposes.# Round--Robin-arbiter
