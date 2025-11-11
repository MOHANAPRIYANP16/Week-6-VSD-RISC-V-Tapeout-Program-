# Week 6 – Inception of open-source EDA, OpenLANE and Sky130 PDK

![Static Badge](https://img.shields.io/badge/OS-linux-darkred)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-darkblue)

This week introduces the fundamentals of chip packaging, RISC-V architecture, SoC design, and the OpenLANE ASIC flow used for creating fabrication-ready layouts.
By the end of this week, you will understand how software maps to silicon and how to run a complete RTL-to-GDS flow using open-source tools.

---

## Table of Contents

- [Introduction to QFN-48 / Chip / Die / IPs](#1-introduction-to-qfn-48-package-chip-pads-core-die-and-ips)  
- [Introduction to RISC-V](#2-introduction-to-risc-v)  
- [Software → Hardware Flow](#3-from-software-applications-to-hardware)  
- [SoC Design and OpenLANE](#4-soc-design-and-openlane)  
- [Simplified RTL2GDS Flow](#5-simplified-rtl2gds-flow)  
- [OpenLANE and Strive Chipsets](#6-openlane-and-strive-chipsets)  
- [Detailed OpenLANE Flow Overview](#7-detailed-openlane-flow-overview)  
- [Lab Work – Environment Setup + Synthesis](#8-lab-work--environment-setup--synthesis)  
- [Task – Flop Ratio / DFF %](#task--flop-ratio--dff-)  
- [Week-6 Summary](#summary)


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

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/chip_structure.png)
---

#### <ins>Foundry & IPs:</ins>
   - **Foundry**: The manufacturing facility where semiconductor chips are fabricated.  
   - **Foundry IPs**: Proprietary design components optimized for a specific process node.  
   - **Macros**: Reusable digital logic blocks.

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/ips.png)

---


## 2. Introduction to RISC-V

RISC-V is an open-source Instruction Set Architecture.

**Key Features**
- No license fees
- Modular / extensible
- Useful for small to high-performance CPU
- Strong open-source support

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/risc_v_architecture.png)

---

## 3. From Software Applications to Hardware

| Layer | Function |
|------|----------|
| Application Software | C / C++ / Python |
| System Software | OS + Compiler + Assembler |
| Hardware | Executes machine binary |

Execution pipeline:
Application → OS → Compiler → Assembler → Binary → Hardware (RTL → Netlist → Layout)

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/software_to_hardware.png)

---

## 4. SoC Design and OpenLANE

To design an SoC we need:

1) RTL (Verilog/VHDL)
2) EDA Tools (Yosys, OpenROAD, Magic, OpenSTA)
3) PDK (Sky130)

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/soc_design.png)

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

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/rtl_to_gdsii_flow.png)

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



---

## 8. Lab Work – Environment Setup + Synthesis

# Environment Setup Guide 

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
![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/ls_opendirectory_dir.png)


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

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/Docker%20Alias%20Setup.png)

### 5. Launch Docker Container

```bash
docker
```
### 6. start OpenLane in interactive mode.

Run inside % :

```bash
./flow.tcl -interactive
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/docker.png)

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

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/load_openlane_package.png)

### 9. Run Synthesis
```bash
run_synthesis
```

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/synthesis.png)

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
![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/total_cells.png)

### DFF Cells Count

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day1/Images/total_dff.png)

---

## Summary

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

