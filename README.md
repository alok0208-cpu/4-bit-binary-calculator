# 4-bit-binary-calculator
A pure hardware calculator built entirely with 74-series TTL logic ICs (no microcontrollers). It features a 4-bit BCD decoder for input, SR memory latches for data storage, a 4-bit binary adder for arithmetic, and a 5-bit BCD decoder to convert the sum and carry bit for 7-segment displays. A complete gate-level digital logic project!

⚙️ Architecture
Input: A 4-Bit BCD Decoder translates user switch inputs into machine-usable logic.

Memory: SR Memory Latches provide volatile storage to hold the first operand during the calculation cycle.

Arithmetic: A 4-Bit Binary Adder computes the sum using cascaded logic gates.

Output: A 5-Bit BCD Decoder converts the 5-bit binary result (4-bit sum + 1-bit carry-out) back into human-readable format for the 7-segment displays.

<img width="688" height="836" alt="Screenshot From 2026-11-11 16-14-53" src="https://github.com/user-attachments/assets/1f701710-eba4-484d-a67b-1e38c52e953c" />

## 🛠️ Hardware Stack

This calculator is constructed using a mix of High-Speed CMOS (74HC series) and 4000-series logic ICs to build the adders, decoders, and memory registers entirely from scratch.

*   **Memory Registers (Sequential Logic):**
    *   **74HC74 (Dual D-Type Flip-Flop):** Used to create stable, edge-triggered memory storage to hold the first operand during the calculation cycle.
*   **Core Logic Gates (Combinational Logic):**
    *   **SN74HC04N / 74HC04 (Hex Inverter):** Provides crucial signal inversion for the BCD decoding stages.
    *   **CD4081BE (Quad 2-Input AND):** Drives the primary logic for computing the 4-bit binary addition.
    *   **3-Input AND Gate IC:** Handles complex, multi-variable logic conditions within the 5-bit BCD output decoder.
    *   **Quad 2-Input OR Gate IC (e.g., 74HC32):** Combines output signals for the adder's carry-out and final display decoding.
*   **I/O & Power:**
    *   7-Segment Displays for output visualization.
    *   DIP switches/push buttons for binary input.
    *   5V DC Power Supply.

 🚀 Operation
Input Operand A: Enter the first digit via the 4-bit BCD interface.

Store: Trigger the SR latch to save the first operand into memory.

Input Operand B: Enter the second digit.

Compute & Display: The 4-bit adder immediately computes the sum of the latched data and the new input. The 5-bit decoder translates the resulting binary sum and carry bit to the 7-segment displays.



