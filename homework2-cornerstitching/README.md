# homework2-cornerstitching

This project implements **macro placement** using the **Genetic Algorithm (GA)** combined with the **Corner Stitching** spatial data structure. It is designed to:

* **Parse macros (pads/terminals)** from Bookshelf files.
* **Determine optimal macro locations** to minimize **Half-Perimeter Wire Length (HPWL)**.
* **Utilize GA and corner-stitching** for efficient macro packing and HPWL evaluation.

The program outputs a `.pl_out` file specifying macro placement and includes scripts for visualization.

## 📄 Input Format

This project supports the **Bookshelf** format and uses the following files:

* **`.aux`** – Index file referencing the circuit description.
* **`.nodes`** – Includes macros and other cells. Only **macros (terminals)** are considered.
* **`.nets`** – Includes nets connecting cells and macros. Only **macro-to-macro nets** are considered.
* **`.pl`** – Placement info including macro orientation and fixed status.
* **`.scl`** – Row structure (used for defining core region boundaries).

📄 **Example `.aux` File**

```
RowBasedPlacement : adaptec1.nodes adaptec1.nets adaptec1.wts adaptec1.pl adaptec1.scl
```

## 📄 Output Format

After execution, the program generates:

* **`.pl_out`** – Macro placement result with all macros marked as `/FIXED`.
* **`.m` (MATLAB)** – Optional script to visualize the placement.
* **`.dat` files** – Optional chip, pad, and row bounding data for Gnuplot.

## 🧰 Project Structure

```
📂 homework2-cornerstitching/
│── 📂 src/ # (main.cpp, GA.cpp, cornerstitching.cpp, parser.cpp, abacus.cpp)
│── 📂 dat/
│── 📂 plt/
│── 🖥️  HW2_StudentID
│── 🔧 Makefile
│── 📜 README.md
│── 📜 .gitignore
../📂 benchmarks/
```

## 🔹 Macro Placement Flow

### **1. Read Input Files**

* `.aux` loads all related paths.
* `.nodes` identifies **macros** (terminals, terminal\_NI).
* `.nets` builds netlists (only macro nets are used).
* `.pl` determines initial macro locations (may be fixed).
* `.scl` extracts the core area.

### **2. Corner Stitching (Placement Engine)**

* **Initialize the corner stitching tile map** with a root tile covering the entire core region.
* For each macro placement attempt:

  * Use `CanPlace()` to ensure the macro can legally occupy a tile region.
  * Insert macro using `CreatSolid()`, which invokes `CutX`, `CutY`, `JoinX`, and `JoinY` as needed to maintain the spatial subdivision.
  * Tiles dynamically track both fixed and newly placed macros.
  * Calculate HPWL based on macro positions.

### **3. Genetic Algorithm (Placement Driver)**

* Initialize a **population** of random macro sequences.
* For each generation:

  * Select parents using **roulette wheel selection**.
  * Perform **crossover** and **mutation** to produce children.
  * Use **corner stitching engine** to validate and score the children by HPWL.
  * Retain best individuals for the next generation.
* Repeat until generation limit is reached.

### **3. Output Generation**

* **`.pl_out`**: Final macro positions with `/FIXED` flag.
* **`.m` file**: MATLAB visualization script using macro and empty tile data.
* Optionally, generate `.dat` files and `.plt` for Gnuplot plotting.

## ⚡ Example Execution

```bash
./HW2_M16131056 ../benchmarks/adaptec1/adaptec1.aux
```

## ✅ Validation & HPWL Calculation

To verify legality and calculate total HPWL:

```bash
python3 tester.py --case adaptec1
```

The tester will:

* Validate that placement is legal (no overlaps, within bounds).
* Report total HPWL, which is used for scoring.

## 🖼️ Visualization

Visual outputs can be generated via:

* **MATLAB** script (`.m`) – Draws colored blocks for placed/fixed macros.
* **Gnuplot** scripts (`.plt`) – Optional row/chip/pad visualization.
