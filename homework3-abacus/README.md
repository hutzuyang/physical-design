# homework3-abacus

This project implements **legalization** using the **Abacus algorithm**, which:

- **Minimizes cell displacement** from global placement.
- **Removes cell overlaps** by aligning them to legal sites.
- **Preserves placement density**.

The program **parses `.gp.pl` (global placement result)**, processes cell movements, and outputs a **legal placement file**.

## 📄 Input Format

This project follows the **Bookshelf** format and requires the following files:

- **`.aux`** - Main index file pointing to `.nodes`, `.nets`, `.gp.pl`, `.scl`, etc.
- **`.nodes`** - Describes circuit components and their dimensions.
- **`.nets`** - Defines netlist connectivity.
- **`.gp.pl`** - **Global placement** results.
- **`.scl`** - Defines row placement structure.

📄 **Example `.aux` File**

```
RowBasedPlacement : circuit.nodes circuit.nets circuit.gp.pl circuit.scl
```

## 📄 Output Format

After execution, the program generates:

- **`.legal.pl`** - Legalized placement file.
- **`.dat`** files - Stores parsed circuit information.
- **`.plt`** files - Gnuplot scripts for visualization.

## 🧰 Project Structure

```
📂 homework3-abacus/
│── 📂 src/ # (main.cpp, parser.cpp, abacus.cpp, and headers)
│── 📂 obj/  
│── 📂 plt/  
│── 📂 dat/  
│── 📂 legal/  
│── 🖥️ HW3_StudentID  
│── 🔧 Makefile
│── 📜 README.md  
│── 📜 .gitignore  

../📂 benchmarks/  
```

## 🔹 **Parsing & Placement Flow**

This project implements **legalization** using the **Abacus algorithm**, a dynamic programming approach that efficiently places standard cells while preserving relative positioning from global placement. The steps are as follows:

### **1. Read Input Files**

- Parse the `.aux` file to obtain the `.nodes`, `.nets`, `.gp.pl`, and `.scl` files.
- Extract **cell dimensions**, **net connections**, and **initial placement positions**.

### **2. Build Data Structures**

- Construct **MODULES**, **NETS**, **PADS**, and **ROWS** data structures.
- Store cell information, connectivity, and row details for placement adjustments.

### **3. Apply the Abacus Algorithm**

- **Sort cells by x-coordinates**: Cells are processed from **left to right** to maintain spatial consistency.
- **Row-based placement**: Each cell is assigned to the closest legal row.
- **Sliding Window Method**:
  - Identify **legal placement sites** using row-based constraints.
  - Assign **minimum displacement** positions for each cell while avoiding overlaps.
  - If a cell overlaps, it is pushed to the nearest **available site**.
- **Row Shifting & Densification**:
  - Adjust cell alignment to ensure **compact placement**.
  - Distribute whitespace efficiently to balance density.

### **4. Output Generation**

- **Generate `.legal.pl` file** with the new legalized positions.
- **Store placement results in `.dat` and `.plt` files** for visualization.

### **5. Validation & HPWL Calculation**

- **Check legality** of placement using `iccad2013_check_legality`.
- **Compute HPWL** to measure placement efficiency using `iccad2013_get_hpwl`.

## ⚡ **Example Execution**

```bash
./HW3_StudentID ../benchmarks/superblue1/superblue1.aux
```

```bash
gnuplot superblue1/superblue1.plt
```

```bash
./iccad2013_check_legality superblue1.aux superblue1.legal.pl
```

```bash
./iccad2013_get_hpwl superblue1.aux superblue1.legal.pl
```

## ✅ Validation & HPWL Calculation

To validate legalization correctness and measure **Half-Perimeter Wirelength (HPWL)**, the following **ICCAD 2013 Checkers** are provided. These tools ensure the legality of the placement and compute the wirelength for performance evaluation.

### **Legality Check**

```bash
./iccad2013_check_legality superblue1.aux superblue1.legal.pl
```

- Ensures that all cells are legally placed without overlaps.

### **HPWL Calculation**

```bash
./iccad2013_get_hpwl superblue1.aux superblue1.legal.pl
```

- Computes **Half-Perimeter Wirelength (HPWL)** for placement evaluation.

## 🖼️ Generated Plots

Below are the generated plots from the `gnuplot` output:

  - superblue1 row
  <img src="https://github.com/user-attachments/assets/f884e112-97ab-4bb5-8caf-6eed0c092c6f" width="50%" height="50%">

  - superblue1 init
  <img src="https://github.com/user-attachments/assets/924b9c2c-e258-491e-9ef9-4f55ddb5a458" width="50%" height="50%">

  - superblue1 legal
  <img src="https://github.com/user-attachments/assets/aa5192d5-ad49-432f-a5fd-59e4827169d9" width="50%" height="50%">  
