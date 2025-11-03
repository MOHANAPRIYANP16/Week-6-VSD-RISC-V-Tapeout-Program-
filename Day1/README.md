# Week 6 – RISC-V Tapeout Program

This week introduces the fundamentals of chip packaging, RISC-V architecture, SoC design, and the OpenLANE ASIC flow used for creating fabrication-ready layouts.
By the end of this week, you will understand how software maps to silicon and how to run a complete RTL-to-GDS flow using open-source tools.

---

## 1. Introduction to QFN-48 Package, Chip, Pads, Core, Die and IPs

A package is the protective case enclosing the actual silicon chip.
The QFN-48 (Quad Flat No-leads) package has 48 electrical connection pins.

**Key Components**
- Pads: Electrical connection points where signals enter/exit the chip
- Core: Contains the functional logic, ALUs, memory
- Die: The silicon that actually contains the circuit
- IPs: Reusable logic blocks like processor cores, peripherals, SRAM, etc.

### <ins>Chip Structure:</ins>
1. **Pads**: Interface points for signals entering or leaving the chip.  
2. **Core**: Contains all the digital logic.  
3. **Die**: The combination of *core + pads* — the fundamental manufacturing unit. <br>

![alt text](<Screenshot 2025-11-03 171553.png>)
---

#### <ins>Foundry & IPs:</ins>
   - **Foundry**: The manufacturing facility where semiconductor chips are fabricated.  
   - **Foundry IPs**: Proprietary design components optimized for a specific process node.  
   - **Macros**: Reusable digital logic blocks.

![alt text](<Screenshot 2025-11-03 171835.png>)

---


## 2. Introduction to RISC-V

RISC-V is an open-source Instruction Set Architecture.

**Key Features**
- No license fees
- Modular / extensible
- Useful for small to high-performance CPU
- Strong open-source support

![alt text](risc_v_architecture.png)

---

## 3. From Software Applications to Hardware

| Layer | Function |
|------|----------|
| Application Software | C / C++ / Python |
| System Software | OS + Compiler + Assembler |
| Hardware | Executes machine binary |

Execution pipeline:
Application → OS → Compiler → Assembler → Binary → Hardware (RTL → Netlist → Layout)

![alt text](software_to_hardware.png)

---

## 4. SoC Design and OpenLANE

To design an SoC we need:

1) RTL (Verilog/VHDL)
2) EDA Tools (Yosys, OpenROAD, Magic, OpenSTA)
3) PDK (Sky130)

![alt text](image.png)

---

## 5. Simplified RTL2GDS Flow

Steps:
- Synthesis
- Floorplan / Power plan
- Placement
- CTS
- Routing
- Sign-off (DRC / LVS / Timing)
- GDSII generation

![alt text](rtl_to_gdsii_flow.png)

---

## 6. OpenLANE and Strive Chipsets

OpenLANE = automated RTL→GDS using Sky130

Features:
- Fully automated
- DFT support
- Runs on Sky130

Strive SoC = RISC-V SoC family built via OpenLANE

---

## 7. Detailed OpenLANE Flow Overview

| Stage | Tool | Purpose |
|-------|------|---------|
| RTL Synthesis | Yosys, ABC | RTL → netlist |
| Physical Implementation | OpenROAD | floorplan, placement, CTS, routing |
| Verification | Magic, Netgen, OpenSTA | DRC/LVS/timing |
| DFT | Fault | scan / ATPG |
| Final Output | GDSII | fab ready |

(IMAGE placeholder: OpenLANE detailed flow)

---

## 8. Lab Work – Environment Setup + Synthesis

# Environment Setup Guide (Improved / Cleaner)

## Overview
This document explains how to set up the OpenLANE environment for the RISC-V Tapeout Program using Oracle VirtualBox.

---

## Method 1 — Using Pre-Configured VDI (Recommended)

### Step 1 — Download VDI
Download the pre-built OpenLANE Virtual Machine image:

https://vsd-labs.sgp1.cdn.digitaloceanspaces.com/vsd-labs/openlane.zip

### Step 2 — Extract
Unzip `openlane.zip` → you will get `openlane.vdi`

### Step 3 — Create New VM in VirtualBox
Open VirtualBox → New → set:
- Name: OpenLANE_Environment
- RAM: 4GB or more
- CPU: 2 cores or more
- DO NOT create a disk yet (we will attach our VDI)

Create the VM.

### Step 4 — Attach .vdi Disk
Settings → Storage  
Controller: IDE → select “Empty” → click disk icon → Choose/Create Virtual Hard Disk → Add → select `openlane.vdi` → Choose → OK

### Step 5 — Start VM
Start the VM → OpenLANE environment boots with all tools pre-installed.

---

## Method 2 — Build From Source (Alternative)

### Step 1 — Clone supporting repository
git clone https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd

This contains flow resources, reference files, designs (ex: picorv32a).

### Step 2 — Build OpenPDKs (Sky130)
git clone https://github.com/RTimothyEdwards/open_pdks.git
cd open_pdks
./configure --enable-sky130-pdk
make
sudo make install

**Note:** Magic ≥ 8.3.530 recommended before building OpenPDKs.

---

## Verification Checklist
- OpenLANE directory exists ✔
- Sky130 PDK folders present in installation path ✔
- Tools (magic, yosys, openroad, etc.) run from terminal ✔

---

## Troubleshooting Hints
- VM doesn’t start → enable virtualization (Intel VT-x / AMD-V) in BIOS
- Disk errors → increase storage allocation in VirtualBox
- Build errors (Method 2) → install missing dependencies (build-essential, tcl, tk, python3-pip, etc.)

### Directory Structure
![alt text](<Screenshot 2025-11-01 141539.png>)


### OpenLANE setup and synthesis :

#### 1. Setting PDK Root Path

```bash
export PDK_ROOT=/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks
```

 - Sets the environment variable `PDK_ROOT` pointing to the Process Design Kit directory containing all the technology files.


### 2. Navigate to OpenLane Directory

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane
```

### 3. Docker Alias Setup:

```bash
alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
```

- This command creates a shortcut `alias` so typing docker actually runs the OpenLane Docker container interactively, mounting your current directory and PDK path, setting environment variables

### 4. Navigate to Designs Folder

```bash
cd designs
```

![alt text](<Screenshot 2025-11-01 142613.png>)

### 5. Launch Docker Container

```bash
docker
```
### 6. start OpenLane in interactive mode.
```bash
./flow.tcl -interactive
```

![alt text](<Screenshot 2025-11-01 142726.png>)

### 7. Load OpenLane Package

Run inside % :

```bash
 package require openlane 0.9
```
This Loads the OpenLane tool package version 0.9 in the Tcl interactive shell.

### 8. Prepare Design

```bash
prep -design picorv32a
```
This  Prepares the design environment for picorv32a processor, setting up necessary files and configurations.

![alt text](<Screenshot 2025-11-01 153723.png>)

### 9. Run Synthesis
```bash
run_synthesis
```

![alt text](<Screenshot 2025-11-01 153925.png>)

- this Executes the logic synthesis step, converting RTL code to gate-level netlist using the specified technology library.

---

##  Task – Flop Ratio / DFF %

| Parameter | Value |
|----------|-------|
| Total Cells | 15134 |
| DFF Cells | 1613  (from `sky130_fd_sc_hd__dfxtp_2`) |

Flop ratio = 1613 / 15134 = 0.1065  
DFF % = 10.65%

### Total Cells Count
![alt text](<Screenshot 2025-11-01 164132.png>)

### DFF Cells Count

![alt text](<Screenshot 2025-11-01 164202.png>)

---

## Summary of Week 6

| No | Topic | Key Focus |
|----|--------|-----------|
| 1 | QFN-48 package & die | physical chip structure |
| 2 | RISC-V ISA | open architecture |
| 3 | Software → hardware flow | mapping software to silicon |
| 4 | SoC design & OpenLANE | components needed |
| 5 | RTL2GDS | overall ASIC steps |
| 6 | Lab synthesis | hands-on synthesis + metrics |

### End of Week-6 Outcomes
- Understand full ASIC flow: RTL → GDSII  
- Can run synthesis in OpenLANE  
- Can calculate flop ratio + DFF % accurately

