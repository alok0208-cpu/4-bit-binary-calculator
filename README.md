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
Since four bits can only represent up to 15, the final carry out ($COUT$) acts as the indispensable fifth, most-significant result bit to prevent overflow.
