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

# 4-Bit Binary Adder (Discrete Logic)

A purely hardware-based 4-bit ripple-carry adder built entirely from fundamental logic gates. Instead of using a dedicated adder IC (like the 74LS83/74HC283) or a microcontroller, this project demonstrates how arithmetic logic units (ALUs) compute addition at the lowest architectural level using AND, OR, and NOT gates.

## ⚙️ Logic Architecture

The 4-bit adder is constructed by cascading individual adder circuits to process 4-bit binary inputs (Operand A and Operand B):
*   **Bit 0 (LSB):** Uses a **Half Adder** (or a Full Adder with Carry-In grounded) to compute the initial sum and carry-out.
*   **Bits 1-3:** Use **Full Adders** that take bit A, bit B, and the Carry-In from the previous stage to calculate the Sum and propagate the Carry-Out.
*   **Bit 4 (Carry-Out):** The final carry-out serves as the 5th bit (MSB) of the result, allowing the circuit to compute sums up to 31 (binary `11111`).

## 🛠️ Hardware Stack

This build utilizes High-Speed CMOS (74HC series) and 4000-series logic ICs to construct the XOR, AND, and OR logic required for binary addition.

*   **CD4081BE:** Quad 2-Input AND Gate (Used for carry generation)
*   **Quad 2-Input OR Gate IC:** (e.g., 74HC32) (Used for carry propagation)
*   **SN74HC04N:** Hex Inverter / NOT Gate (Used in combination with AND/OR gates to construct XOR logic if dedicated XOR ICs are not used)
*   **Inputs:** 8-position DIP switch for entering 4-bit Operand A and 4-bit Operand B.
*   **Outputs:** 5 LEDs (4 for the sum, 1 for the final carry-out).

![Uploading adder_4bit_ripple_carry.svg…]()
