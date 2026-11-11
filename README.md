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
    *   
🚀 Operation

Input Operand A: Enter the first digit via the 4-bit BCD interface.

Store: Trigger the SR latch to save the first operand into memory.

Input Operand B: Enter the second digit.

Compute & Display: The 4-bit adder immediately computes the sum of the latched data and the new input. The 5-bit decoder translates the resulting binary sum and carry bit to the 7-segment displays.

**4-bit binary to decimal converter for seven segment display**

The input is represented by four bits:

| Input | Decimal |
|---|---:|
| 0000 | 0 |
| 0001 | 1 |
| 0010 | 2 |
| 0011 | 3 |
| 0100 | 4 |
| 0101 | 5 |
| 0110 | 6 |
| 0111 | 7 |
| 1000 | 8 |
| 1001 | 9 |
| 1010 | 10 |
| 1011 | 11 |
| 1100 | 12 |
| 1101 | 13 |
| 1110 | 14 |
| 1111 | 15 |

## Ones Digit Logic

Let A, B, C and D represent the binary inputs with weights 8, 4, 2 and 1.

The BCD output equations are:

- Y3 = A·B'·C'
- Y2 = B·(A' + C)
- Y1 = A'·C + A·B·C'
- Y0 = D

These equations produce the ones digit for inputs 0–15.

# SR Memory Latch – Digital Calculator

## Overview

This module implements an SR (Set-Reset) Memory Latch using digital logic ICs and electronic components.

The SR latch is a fundamental sequential logic circuit capable of storing one bit of binary information. Unlike combinational logic circuits, which depend only on current inputs, an SR latch can retain its previous state.

In this project, the SR latch is used as a temporary memory unit in the hardware-based digital calculator to store binary input data before arithmetic operations.

## Features

- Stores one bit of binary information.
- Implements Set and Reset operations.
- Retains its previous state.
- Built using fundamental digital logic.
- No microcontroller or programming required.
- Integrated into a hardware-based calculator.

## 1. Working Principle

An SR latch has two control inputs:

- **S (Set):** Sets the output to logic HIGH.
- **R (Reset):** Resets the output to logic LOW.

It has two outputs:

- Q
- Q̅ (complement of Q)

The circuit maintains its output state until a valid Set or Reset operation occurs.

## 2. Truth Table

For an active-HIGH NOR-based SR latch:

| S | R | Q(next) | Operation |
|---|---|---|---|
| 0 | 0 | Previous Q | Memory / Hold |
| 0 | 1 | 0 | Reset |
| 1 | 0 | 1 | Set |
| 1 | 1 | Invalid | Forbidden state |

**Note:** The simultaneous HIGH condition on both inputs is forbidden for a NOR-based SR latch.

## 3. Circuit Architecture

```text
       SET (S)
          |
          v
     +----------+
     |          |
     |  NOR     |<--------+
     |  GATE 1  |         |
     +----------+         |
          |               |
          v               |
          Q               |
                          |
                       +------+
                       |      |
                       | NOR  |
                       | GATE |
                       |  2   |
                       +------+
                          |
                          v
                         Q̅
                          |
                          +------+
                                 |
                           RESET (R)
```

The two cross-coupled NOR gates provide the feedback required for storing the previous output state.

# 4-Bit Ripple-Carry Binary Adder

A purely hardware-based 4-bit binary adder built entirely from discrete logic gates. It adds two 4-bit unsigned binary numbers and produces a 5-bit result using four cascaded full-adder stages.

## 🔌 1. Inputs and Outputs

**Inputs:**
* **Operand A:** `A3 A2 A1 A0`
* **Operand B:** `B3 B2 B1 B0`
* **Initial Carry:** `C0 = 0` (Tied LOW)

**Outputs:**
* **Sum:** `SUM3 SUM2 SUM1 SUM0`
* **Final Carry:** `COUT` (Serves as the Most Significant Bit)
* **Complete 5-bit Result:** `COUT SUM3 SUM2 SUM1 SUM0`

---

## 🏗️ 2. Block Diagram

The carry output of each stage feeds directly into the carry input of the next stage. This arrangement is known as a **ripple-carry adder**.

* `A0, B0, C0=0` ➔ **Full Adder 0** ➔ `SUM0, C1`
* `A1, B1, C1  ` ➔ **Full Adder 1** ➔ `SUM1, C2`
* `A2, B2, C2  ` ➔ **Full Adder 2** ➔ `SUM2, C3`
* `A3, B3, C3  ` ➔ **Full Adder 3** ➔ `SUM3, COUT`

---

## 🧩 3. Hardware Gates Used

Each full-adder stage is constructed from combining two half-adders:
* **2** XOR gates
* **2** AND gates
* **1** OR gate

**Total for 4 Stages:** 8 XOR gates, 8 AND gates, and 4 OR gates.

---

## 🧮 4. Logic Theory & Equations

### Half-Adder
A half-adder adds two single-bit inputs, A and B.
* **Sum** (HIGH when inputs differ): $S = A \oplus B$
* **Carry** (HIGH when both inputs are HIGH): $C = A \cdot B$

### Full-Adder
A full-adder combines three bits: A, B, and the Carry-In ($Cin$) from the previous stage. By combining two half-adders, the equations are:

* **Sum:** 
  $$SUM = A \oplus B \oplus Cin$$
* **Carry-Out:** 
  $$Cout = AB + Cin(A \oplus B)$$
  *(Equivalent to: $Cout = AB + ACin + BCin$)*

**Full-Adder Truth Table:**

| A | B | Cin | SUM | Cout |
|---|---|-----|-----|------|
| 0 | 0 | 0   | 0   | 0    |
| 0 | 0 | 1   | 1   | 0    |
| 0 | 1 | 0   | 1   | 0    |
| 0 | 1 | 1   | 0   | 1    |
| 1 | 0 | 0   | 1   | 0    |
| 1 | 0 | 1   | 0   | 1    |
| 1 | 1 | 0   | 0   | 1    |
| 1 | 1 | 1   | 1   | 1    |

---

## 📊 5. Four-Bit Adder Stage Equations

Setting the initial carry $C_0 = 0$, the propagation ($P_i$) and generation for each bit ($i = 0, 1, 2, 3$) is evaluated as:

* **Bit 0:** $P_0 = A_0 \oplus B_0$ | $SUM_0 = P_0 \oplus C_0$ | $C_1 = A_0B_0 + P_0C_0$
* **Bit 1:** $P_1 = A_1 \oplus B_1$ | $SUM_1 = P_1 \oplus C_1$ | $C_2 = A_1B_1 + P_1C_1$
* **Bit 2:** $P_2 = A_2 \oplus B_2$ | $SUM_2 = P_2 \oplus C_2$ | $C_3 = A_2B_2 + P_2C_2$
* **Bit 3:** $P_3 = A_3 \oplus B_3$ | $SUM_3 = P_3 \oplus C_3$ | $C_4 = A_3B_3 + P_3C_3$

The final carry is $COUT = C_4$.

---

## ❓ 6. Why is the output five bits?

A 4-bit unsigned binary number ranges from 0 to 15 ($1111_2$). 
The largest possible sum of two 4-bit numbers is:
$$15 + 15 = 30$$
In binary, $30_{10}$ is equal to $11110_2$. 
Since four bits can only represent up to 15, the final carry out ($COUT$) acts as the indispensable fifth, most-significant result bit to prevent overflow

<svg xmlns="http://www.w3.org/2000/svg" width="1300" height="1160" viewBox="0 0 1300 1160">
<defs><style>
.w{fill:none;stroke:#172033;stroke-width:3;stroke-linecap:round;stroke-linejoin:round}.g{fill:#fff;stroke:#172033;stroke-width:3}
.t{font:bold 30px Arial;fill:#101827}.h{font:bold 21px Arial;fill:#2457a6}.l{font:bold 20px Arial;fill:#172033}.sm{font:17px Arial;fill:#172033}.c{fill:none;stroke:#c2410c;stroke-width:3.5;stroke-linecap:round;stroke-linejoin:round}
</style></defs><rect width="1300" height="1160" fill="white"/>
<text x="650" y="45" text-anchor="middle" class="t">4-BIT RIPPLE-CARRY ADDER — GATE-LEVEL CIRCUIT</text>
<text x="650" y="76" text-anchor="middle" class="sm">Each stage: two XOR gates, two AND gates and one OR gate</text>
<text x="35" y="143" class="h">FULL ADDER 0 (BIT 0)</text><text x="35" y="193" class="l">A0</text><text x="35" y="243" class="l">B0</text><path class="w" d="M75 185 H205 M75 235 H205"/><path class="w" d="M205 163 Q232 210 205 257 M216 163 Q243 210 216 257 Q260 210 216 163"/><text x="260" y="155" class="sm">P0</text><path class="w" d="M260 210 H405"/><path class="w" d="M405 163 Q432 210 405 257 M416 163 Q443 210 416 257 Q460 210 416 163"/><path class="w" d="M460 210 H590"/><text x="600" y="217" class="l">SUM0</text><path class="w" d="M95 185 V285 H205 M95 235 V285"/><path class="g" d="M205 265 H245 A22 22 0 0 1 245 309 H205 Z"/><text x="275" y="330" class="sm">G0=A0·B0</text><path class="w" d="M330 210 V285 H465"/><text x="340" y="320" class="sm">C0 = 0</text><path class="c" d="M390 315 H405 V240 H465 V287"/><path class="g" d="M465 265 H505 A22 22 0 0 1 505 309 H465 Z"/><text x="520" y="330" class="sm">H0=P0·C0</text><path class="w" d="M267 287 H690 V273 H730 M527 287 H690 V301 H730"/><path class="w" d="M730 260 Q758 287 730 314 Q790 287 730 260"/><path class="w" d="M790 287 H950"/><text x="965" y="294" class="l">C1</text><path class="c" d="M950 287 H1120 V465 H405"/><text x="35" y="368" class="h">FULL ADDER 1 (BIT 1)</text><text x="35" y="418" class="l">A1</text><text x="35" y="468" class="l">B1</text><path class="w" d="M75 410 H205 M75 460 H205"/><path class="w" d="M205 388 Q232 435 205 482 M216 388 Q243 435 216 482 Q260 435 216 388"/><text x="260" y="380" class="sm">P1</text><path class="w" d="M260 435 H405"/><path class="w" d="M405 388 Q432 435 405 482 M416 388 Q443 435 416 482 Q460 435 416 388"/><path class="w" d="M460 435 H590"/><text x="600" y="442" class="l">SUM1</text><path class="w" d="M95 410 V510 H205 M95 460 V510"/><path class="g" d="M205 490 H245 A22 22 0 0 1 245 534 H205 Z"/><text x="275" y="555" class="sm">G1=A1·B1</text><path class="w" d="M330 435 V510 H465"/><text x="340" y="545" class="sm">C1</text><path class="c" d="M405 465 H465 V512"/><path class="g" d="M465 490 H505 A22 22 0 0 1 505 534 H465 Z"/><text x="520" y="555" class="sm">H1=P1·C1</text><path class="w" d="M267 512 H690 V498 H730 M527 512 H690 V526 H730"/><path class="w" d="M730 485 Q758 512 730 539 Q790 512 730 485"/><path class="w" d="M790 512 H950"/><text x="965" y="519" class="l">C2</text><path class="c" d="M950 512 H1120 V690 H405"/><text x="35" y="593" class="h">FULL ADDER 2 (BIT 2)</text><text x="35" y="643" class="l">A2</text><text x="35" y="693" class="l">B2</text><path class="w" d="M75 635 H205 M75 685 H205"/><path class="w" d="M205 613 Q232 660 205 707 M216 613 Q243 660 216 707 Q260 660 216 613"/><text x="260" y="605" class="sm">P2</text><path class="w" d="M260 660 H405"/><path class="w" d="M405 613 Q432 660 405 707 M416 613 Q443 660 416 707 Q460 660 416 613"/><path class="w" d="M460 660 H590"/><text x="600" y="667" class="l">SUM2</text><path class="w" d="M95 635 V735 H205 M95 685 V735"/><path class="g" d="M205 715 H245 A22 22 0 0 1 245 759 H205 Z"/><text x="275" y="780" class="sm">G2=A2·B2</text><path class="w" d="M330 660 V735 H465"/><text x="340" y="770" class="sm">C2</text><path class="c" d="M405 690 H465 V737"/><path class="g" d="M465 715 H505 A22 22 0 0 1 505 759 H465 Z"/><text x="520" y="780" class="sm">H2=P2·C2</text><path class="w" d="M267 737 H690 V723 H730 M527 737 H690 V751 H730"/><path class="w" d="M730 710 Q758 737 730 764 Q790 737 730 710"/><path class="w" d="M790 737 H950"/><text x="965" y="744" class="l">C3</text><path class="c" d="M950 737 H1120 V915 H405"/><text x="35" y="818" class="h">FULL ADDER 3 (BIT 3)</text><text x="35" y="868" class="l">A3</text><text x="35" y="918" class="l">B3</text><path class="w" d="M75 860 H205 M75 910 H205"/><path class="w" d="M205 838 Q232 885 205 932 M216 838 Q243 885 216 932 Q260 885 216 838"/><text x="260" y="830" class="sm">P3</text><path class="w" d="M260 885 H405"/><path class="w" d="M405 838 Q432 885 405 932 M416 838 Q443 885 416 932 Q460 885 416 838"/><path class="w" d="M460 885 H590"/><text x="600" y="892" class="l">SUM3</text><path class="w" d="M95 860 V960 H205 M95 910 V960"/><path class="g" d="M205 940 H245 A22 22 0 0 1 245 984 H205 Z"/><text x="275" y="1005" class="sm">G3=A3·B3</text><path class="w" d="M330 885 V960 H465"/><text x="340" y="995" class="sm">C3</text><path class="c" d="M405 915 H465 V962"/><path class="g" d="M465 940 H505 A22 22 0 0 1 505 984 H465 Z"/><text x="520" y="1005" class="sm">H3=P3·C3</text><path class="w" d="M267 962 H690 V948 H730 M527 962 H690 V976 H730"/><path class="w" d="M730 935 Q758 962 730 989 Q790 962 730 935"/><path class="w" d="M790 962 H950"/><text x="965" y="969" class="l">COUT = C4</text><text x="55" y="1030" class="l">Pᵢ = Aᵢ XOR Bᵢ</text>
<text x="430" y="1030" class="l">SUMᵢ = Pᵢ XOR Cᵢ</text>
<text x="55" y="1070" class="l">Gᵢ = Aᵢ AND Bᵢ</text>
<text x="430" y="1070" class="l">Hᵢ = Pᵢ AND Cᵢ</text>
<text x="55" y="1110" class="l">Cᵢ₊₁ = Gᵢ OR Hᵢ</text>
<text x="650" y="1110" class="sm">5-bit result = COUT · SUM3 · SUM2 · SUM1 · SUM0</text></svg>

<img width="1300" height="1160" alt="adder_4bit_ripple_carry" src="https://github.com/user-attachments/assets/644c6e06-25c4-4a93-a070-7a76a3d594dd" />

