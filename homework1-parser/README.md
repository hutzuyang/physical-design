# homework1-parser

This project implements a **Bookshelf parser and placement visualizer**. It is designed to:

* **Parse circuit design files** in Bookshelf format.
* **Extract and process module, pad, pin, and net data**.
* **Visualize the floorplan** including core region, cells, and I/O pads.

The program processes input files (`.aux`, `.nodes`, `.nets`, `.pl`, `.scl`) and generates:

* **`.dat` files** for visualization.
* **`.plt` file** for Gnuplot plotting.

## 📄 Input Format

This project supports the **Bookshelf** format and requires:

* **`.aux`** – Entry point that references other Bookshelf files.
* **`.nodes`** – Lists all modules (standard cells, macros) and pads with dimensions.
* **`.nets`** – Lists all nets and their connected pins with offsets.
* **`.pl`** – Provides placement (x, y, orientation) for each module/pad.
* **`.scl`** – Describes row placement constraints.

📄 **Example `.aux` File**

```
RowBasedPlacement : adaptec1.nodes adaptec1.nets adaptec1.wts adaptec1.pl adaptec1.scl
```

## 📄 Output Format

After execution, the program outputs:

* **`*_chip.dat`** – Core/die region coordinates.
* **`*_cell.dat`** – All standard cell/macro bounding boxes.
* **`*_pad.dat`** – Pad bounding boxes with directional arrows.
* **`*_pad_pin.dat`** – Pin positions on pads.
* **`*.plt`** – Gnuplot script for visualization.

## 🧰 Project Structure

```
📂 homework1-parser/
│── 📂 src/ # (main.cpp, parser.cpp/h, datatype.cpp/h)
│── 📂 dat/
│── 🖥️ HW1_StudentID
│── 🔧 Makefile
│── 📜 README.md
│── 📜 .gitignore
../📂 benchmarks/
```

## 🔹 **Parsing & Output Flow**

### **1. Read Input Files**

* Parse `.aux` to retrieve paths to `.nodes`, `.nets`, `.pl`, `.scl`.
* Extract:

  * **Cell dimensions and types** from `.nodes`
  * **Netlist connections and pin offsets** from `.nets`
  * **Placement positions and orientations** from `.pl`
  * **Core region boundaries** from `.scl`

### **2. Build Data Structures**

* Populate `MODULES`, `NETS`, `PINS`, `PADS`, and `ROWS` vectors.
* Store geometric data using `FPOS`, `RECT`, and other primitives.

### **3. Generate Output**

* `*_chip.dat`: Die/core region
* `*_cell.dat`: Cell rectangles (with respect to chip origin)
* `*_pad.dat`: Pad outlines and direction arrows (N/W/S/E)
* `*_pad_pin.dat`: Pin boxes drawn on pads (3x3 square)
* `*.plt`: Plots everything via `gnuplot`

### **4. Gnuplot Visualization**

* Visualize floorplan and pad placements using `*.plt`.

## ⚡ **Example Execution**

```bash
./HW1_M16131056 ../benchmarks/adaptec1/adaptec1.aux
```

```bash
gnuplot dat/adaptec1/adaptec1.plt
```

## 🖼️ Sample Visualization

  - adaptec1
  <img src="https://github.com/user-attachments/assets/2a8ac49e-d25f-46d1-a576-a89352dee4da" width="50%" height="50%">
  
  - adaptec2
  <img src="https://github.com/user-attachments/assets/20c2e3fe-e268-49d3-bdda-7990c3bfbdd5" width="50%" height="50%">
  
  - adaptec3
  <img src="https://github.com/user-attachments/assets/cb919408-aacf-4533-bdd5-d4397e2af5c4" width="50%" height="50%">
  
  - adaptec4
  <img src="https://github.com/user-attachments/assets/82202eec-832c-4bfc-b753-97dd0ed9dd6e" width="50%" height="50%">
  
  - bigblue1
  <img src="https://github.com/user-attachments/assets/e91da0c4-cc6c-4ee9-a33d-3b3bf54763ac" width="50%" height="50%">
  
  - bigblue2
  <img src="https://github.com/user-attachments/assets/be7889b4-2025-4a89-a50a-d94360b0d3d3" width="50%" height="50%">
  
  - bigblue3
  <img src="https://github.com/user-attachments/assets/59d8144e-a757-4dc0-987d-dc4f1bca5d34" width="50%" height="50%">
  
  - bigblue4
  <img src="https://github.com/user-attachments/assets/5654ad12-a2d9-4193-ab69-b6fea60232e2" width="50%" height="50%">
  
