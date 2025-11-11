# Day 2: Good Floorplan vs Bad Floorplan and Introduction to Library Cells

![Static Badge](https://img.shields.io/badge/OS-linux-darkred)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-darkblue)

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
![alt text](<synhesisand floorplan.png>)

![alt text](pdnsuccessful.png)

2. Calculate the die area in microns from the values in the floorplan def:

Screenshot of contents of floorplan def

![alt text](calculation_day2.png)

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

![alt text](image.png)

![alt text](image-1.png)

Decap Cells and Tap Cells:

![alt text](image-2.png)

4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

```bash
# Now we can run placement 
run_placement
```
workflow:
![alt text](image-3.png)

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
![alt text](image-4.png)

Standard cells legally placed

![alt text](image-5.png)

Commands to exit from current run
```bash
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
