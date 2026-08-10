# 3-to-8 Decoder Using NAND Gates

## 📌 Project Overview

This project implements a **3-to-8 Decoder using NAND gates** in Verilog HDL.

Unlike a behavioral decoder that uses a `case` statement, this project demonstrates the decoder at the **gate level**.

NAND gates are used to implement:

* NOT operations
* Decoder minterms
* Active-low outputs

---

## 🎯 Objective

The objective is to understand how a 3-to-8 decoder can be constructed using NAND gates.

The project demonstrates:

* Gate-level Verilog
* Universal NAND gate
* Active-low logic
* Binary decoding
* Combinational logic
* Testbench verification

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog
* ModelSim / QuestaSim
* GTKWave (optional)
* GitHub

---

## 📂 Project Structure

```text
3-to-8-Decoder-NAND-Gates/
│
├── README.md
├── src/
│   └── decoder_3to8_nand.v
│
├── testbench/
│   └── tb_decoder_3to8_nand.v
│
└── simulation/
    └── simulation_results.txt
```

---

## 🔢 Inputs and Outputs

| Signal | Width | Description                |
| ------ | ----: | -------------------------- |
| A      | 1-bit | Input bit                  |
| B      | 1-bit | Input bit                  |
| C      | 1-bit | Input bit                  |
| Y      | 8-bit | Active-low decoded outputs |

---

## 🧠 Why NAND Gates?

NAND is known as a **universal logic gate** because any basic logic operation can be constructed using NAND gates.

For example, a NOT gate can be created by connecting both inputs of a NAND gate together:

```verilog
nand (a_bar, a, a);
```

This produces:

```text
a_bar = NOT(a)
```

---

## ⚙️ Decoder Operation

The decoder generates one active-low output for each binary input combination.

### Truth Table

|  A |  B |  C | Active Output |
| -: | -: | -: | ------------- |
|  0 |  0 |  0 | Y0            |
|  0 |  0 |  1 | Y1            |
|  0 |  1 |  0 | Y2            |
|  0 |  1 |  1 | Y3            |
|  1 |  0 |  0 | Y4            |
|  1 |  0 |  1 | Y5            |
|  1 |  1 |  0 | Y6            |
|  1 |  1 |  1 | Y7            |

Because the outputs are active LOW, the selected output becomes `0`.

---

## 🔥 Example

For:

```text
A B C = 101
```

The selected output is:

```text
Y5
```

Therefore:

```text
Y = 11011111
```

Here:

```text
Y5 = 0
```

and all other outputs are `1`.

---

## 🔌 Active-Low Logic

An active-low signal is considered active when its value is:

```text
0
```

Therefore:

```text
Y5 = 0
```

means output 5 is selected.

This type of logic is widely used in digital electronics.

---

## 🧪 Testbench

The testbench verifies all **8 possible combinations** of the three input signals.

Test cases:

```text
000
001
010
011
100
101
110
111
```

---

## ▶️ Simulation Using Icarus Verilog

Open the terminal inside the project directory.

### Compile

```bash
iverilog -o decoder_nand_sim src/decoder_3to8_nand.v testbench/tb_decoder_3to8_nand.v
```

### Run

```bash
vvp decoder_nand_sim
```

---

## 📋 Expected Output

```text
================================================
       3-TO-8 NAND GATE DECODER TEST
================================================
 INPUT | OUTPUT
-----------------------------------------------
  000  | 11111110
  001  | 11111101
  010  | 11111011
  011  | 11110111
  100  | 11101111
  101  | 11011111
  110  | 10111111
  111  | 01111111
================================================
```

---

## 📚 Concepts Demonstrated

* NAND gate
* Universal gates
* Gate-level modeling
* Decoder
* Active-low logic
* NOT using NAND
* Minterms
* Combinational circuits
* Testbench development
* Functional verification

---

## 🚀 Applications

Decoder circuits are used in:

* Memory address decoding
* Instruction decoding
* Chip selection
* Digital control systems
* Processor architectures
* Display systems
* Demultiplexing

---

## 🚀 Future Improvements

This project can be extended to:

* 4-to-16 NAND decoder
* NAND-only ALU components
* NAND-based multiplexer
* NAND-based full adder
* FPGA implementation
* CMOS NAND gate implementation

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module decoder_3to8_nand (
    input  wire a,
    input  wire b,
    input  wire c,
    output wire [7:0] y
);

    wire a_bar;
    wire b_bar;
    wire c_bar;

    // NOT gates using NAND gates
    nand (a_bar, a, a);
    nand (b_bar, b, b);
    nand (c_bar, c, c);

    // 3-to-8 decoder using NAND gates
    nand (y[0], a_bar, b_bar, c_bar);
    nand (y[1], a_bar, b_bar, c);
    nand (y[2], a_bar, b, c_bar);
    nand (y[3], a_bar, b, c);
    nand (y[4], a, b_bar, c_bar);
    nand (y[5], a, b_bar, c);
    nand (y[6], a, b, c_bar);
    nand (y[7], a, b, c);

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_decoder_3to8_nand;

    reg a;
    reg b;
    reg c;

    wire [7:0] y;

    decoder_3to8_nand DUT (
        .a(a),
        .b(b),
        .c(c),
        .y(y)
    );

    task test_input;
        input test_a;
        input test_b;
        input test_c;

        begin
            a = test_a;
            b = test_b;
            c = test_c;

            #10;

            $display(
                "INPUT=%b%b%b | OUTPUT=%b",
                a, b, c, y
            );
        end
    endtask

    initial begin

        $display("================================================");
        $display("       3-TO-8 NAND GATE DECODER TEST");
        $display("================================================");
        $display(" INPUT | OUTPUT");
        $display("-----------------------------------------------");

        // All possible input combinations

        test_input(1'b0, 1'b0, 1'b0);
        test_input(1'b0, 1'b0, 1'b1);
        test_input(1'b0, 1'b1, 1'b0);
        test_input(1'b0, 1'b1, 1'b1);
        test_input(1'b1, 1'b0, 1'b0);
        test_input(1'b1, 1'b0, 1'b1);
        test_input(1'b1, 1'b1, 1'b0);
        test_input(1'b1, 1'b1, 1'b1);

        $display("================================================");

        $finish;

    end

endmodule
```
# 3-TO-8 NAND GATE DECODER SIMULATION RESULTS

## INPUT | OUTPUT

000  | 11111110
001  | 11111101
010  | 11111011
011  | 11110111
100  | 11101111
101  | 11011111
110  | 10111111
111  | 01111111

================================================
Simulation completed successfully.
==================================
