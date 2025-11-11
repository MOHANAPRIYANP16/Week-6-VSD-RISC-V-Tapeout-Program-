# Day 2: Good Floorplan vs Bad Floorplan and Introduction to Library Cells

![Static Badge](https://img.shields.io/badge/OS-linux-darkred)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-darkblue)

## 📚 Table of Contents

| Section | Title | Description |
|----------|--------|-------------|
| 1 | **Running Floorplan for `picorv32a` in OpenLANE** | Steps to invoke OpenLANE in interactive mode, prepare the design, and run synthesis and floorplan. |
| 2 | **Calculating Die Area in Microns** | Calculation of die width, height, and area using values from the floorplan DEF file. |
| 3 | **Loading Floorplan DEF in Magic** | Commands to visualize the generated floorplan in Magic and explore decap and tap cells. |
| 4 | **Running Congestion-Aware Placement** | Execution of placement stage in OpenLANE and understanding of placement workflow. |
| 5 | **Exploring Placement in Magic** | Loading and analyzing placement DEF in Magic to verify standard cell placement. |
| 6 | **Exiting OpenLANE and Docker Environment** | Commands to safely exit OpenLANE interactive mode and Docker subsystem. |

1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan


```
![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/synhesisand%20floorplan.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/pdnsuccessful.png)

2. Calculate the die area in microns from the values in the floorplan def:

Screenshot of contents of floorplan def

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/calculation_day2.png)

According to floorplan def
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```

#### 3. Load generated floorplan def in magic tool and explore the floorplan.

> [!Note]
> Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<your_new_run_folder>/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
Screenshots of floorplan def in magic:

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/floorplan.png)

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/floorplan1.png)

Decap Cells and Tap Cells:

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/decap_cells.png)

4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

```bash
# Now we can run placement 
run_placement
```
workflow:
![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/run_placement.png)

5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal which doesnt uses docker :

```bash
# Change directory to path containing generated placement def
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<your_new_run_folder>/results/placement
#like this the your_new_run_folder 11-11_15-16

# Command to load the placement def in magic tool
magic -T /openLANE_flow/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef \
def read picorv32a.placement.def &
```
![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/placement_magic.png)

Standard cells legally placed

![alt text](https://github.com/MOHANAPRIYANP16/Week-6-VSD-RISC-V-Tapeout-Program-/blob/main/Day2/Images/standard_cells_legacy.png)

Commands to exit from current run
```bash
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
## 🧾 Summary

On **Day 2**, the focus was on understanding the **difference between good and bad floorplans**, exploring **library cells**, and using the **OpenLANE flow** to generate and analyze the **floorplan** and **placement** of the `picorv32a` RISC-V core design.

The session began by setting up and invoking the **OpenLANE interactive flow** inside Docker. After preparing the design, **synthesis** and **floorplanning** were executed, followed by calculation of the **die area** in microns using data from the DEF file.

The generated floorplan was visualized in **Magic**, allowing exploration of **decap cells** and **tap cells**, which ensure proper power distribution and substrate biasing.  
Next, a **congestion-aware placement** was performed, and the resulting placement DEF was loaded in Magic to verify that **standard cells** were legally placed.

By the end of this exercise, you learned to:
- Run synthesis, floorplanning, and placement using OpenLANE  
- Calculate die area using DEF file parameters  
- Visualize layouts using Magic  
- Understand library cell usage in ASIC design  

---