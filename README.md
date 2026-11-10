# 🛠️ How AVR Microcontroller Registers Work (Line-by-Line)

When you write basic Arduino code to turn on an LED, it hides a lot of binary overhead (~1 KB to 2 KB). By using **AVR Assembly Language**, we can achieve the exact same result in just **59 bytes** of binary!

Here is exactly what happens inside the **ATmega328P microcontroller** registers line-by-line.

---

## The Code

```assembly
.include "m328pdef.inc" 
.cseg 
.org 0x00 

ldi r16, 0b00100000   ; Line 1: Load data into register
out DDRB, r16         ; Line 2: Set pin direction
out PORTB, r16        ; Line 3: Turn pin HIGH

loop: 
    rjmp loop         ; Line 4: Infinite loop
```

---

## 🔍 Line-by-Line Execution Breakdown

![LDI Animation](ldi.gif)

### 1. `ldi r16, 0b00100000`
* **Command:** `ldi` (Load Immediate).
* **What it does:** Loads the raw binary number `00100000` directly into a temporary workspace called **Register 16 (r16)**.
* **Hardware State:** 
  * `r16` = `00100000`
  * This specific binary configuration targets the **5th bit (b5)**, which corresponds to Digital Pin 13 on an Arduino Uno.

![DDRB Animation](ddrb.gif)

### 2. `out DDRB, r16`
* **Command:** `out` (Output to I/O Register).
* **What it does:** Copies the contents of `r16` into **DDRB** (Data Direction Register for Port B). 
* **Hardware State:** 
  * Setting a bit to `1` in the direction register configures that specific pin as an **Output**. 
  * Pin 13 is now officially set to output mode.

![PORTB Animation](portb.gif)

### 3. `out PORTB, r16`
* **Command:** `out` (Output to I/O Register).
* **What it does:** Copies the contents of `r16` into **PORTB** (Output Register for Port B).
* **Hardware State:** 
  * The voltage at Pin 13 shifts directly from **0V (LOW) to 5V (HIGH)**.
  * If an LED is connected to Pin 13, it turns on right at this moment!

### 4. `loop: rjmp loop`
* **Command:** `rjmp` (Relative Jump).
* **What it does:** Tells the program counter to jump backwards to the label named `loop`.
* **Hardware State:** 
  * Creates an infinite loop. This keeps the microcontroller running safely in place so it doesn't try to execute empty space in the program memory.

---

## 📝 Quick Reference for Beginners

* **DDRx (Direction Register):** Sets whether a pin is an Input (`0`) or an Output (`1`).
* **PORTx (Output Register):** Sets the physical voltage level. Writing `0` outputs `0V`, writing `1` outputs `5V`.
* **PINx (Input Register):** Reads the incoming external voltage level from the physical pin.

