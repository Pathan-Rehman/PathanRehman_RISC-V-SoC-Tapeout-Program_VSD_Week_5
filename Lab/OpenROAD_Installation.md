OpenROAD is an open-source, fully automated RTL-to-GDSII flow for digital integrated circuit (IC) design. It supports synthesis, floorplanning, placement, clock tree synthesis, routing, and final layout generation. OpenROAD enables rapid design iterations, making it ideal for academic research and industry prototyping.

### `Steps to Install OpenROAD and Run GUI`

### 1. Clone the OpenROAD Repository

## 🧩 Step 1: Install Prerequisites
Update your system and install core build tools:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential cmake clang g++ gcc git python3 python3-dev \
  libboost-all-dev libtcl tcl-dev tcllib libreadline-dev zlib1g-dev flex bison \
  swig libpcre3-dev qtbase5-dev liblemon-dev libspdlog-dev libeigen3-dev libffi-dev \
  pkg-config libjson-c-dev libzstd-dev
```
<img width="731" height="506" alt="image" src="https://github.com/user-attachments/assets/a6247bdd-997c-41ba-8a70-10a1c0503002" />


## 📦 Step 2: Clone the Repositories
Install OpenROAD-flow scripts (wrapper for Yosys, OpenROAD, etc.):

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts.git
cd OpenROAD-flow-scripts
```

<img width="735" height="550" alt="image" src="https://github.com/user-attachments/assets/488efc4e-9b5d-4af6-9bfd-5cb4748fbe69" />


## ⚙️ Step 3: Run the Setup Script
Run the setup installer (this installs all required third-party libraries):

```bash
sudo ./setup.sh
```
This step sets up everything OpenROAD depends on — including Boost, SWIG, Abseil, and more.

<img width="733" height="553" alt="image" src="https://github.com/user-attachments/assets/8037084b-dfee-4aee-b488-33230373b7da" />


## 🏗️ Step 4: Build OpenROAD Locally
Now build OpenROAD itself using the automated build script:

```bash
./build_openroad.sh --local
```
💡 This step takes about 30–45 minutes depending on cores and RAM.

<img width="739" height="561" alt="image" src="https://github.com/user-attachments/assets/7240918e-381d-41dd-9530-e4635b434222" />

If tests fail to build (common Google Test issue), you can skip them:

```bash
./build_openroad.sh --local --disable-tests
```

<img width="742" height="553" alt="image" src="https://github.com/user-attachments/assets/00d86fd0-4f8e-4be0-99d1-b92f6a095070" />


## Step 5: Verify Installation

```bash
source ./env.sh
yosys -help  
openroad -help
yosys --version
openroad --version
verilator --version
```

<img width="739" height="553" alt="image" src="https://github.com/user-attachments/assets/45f0116e-f618-4a7c-89fa-eccd548c3f65" />


## Step 6: Run the OpenROAD Flow

```bash
cd flow
make
```

<img width="739" height="557" alt="image" src="https://github.com/user-attachments/assets/8e6ab8eb-8031-4e7d-a1c4-c45c9d997cac" />


## Step 7. Launch the graphical user interface (GUI) to visualize the final layout

```bash
 make gui_final
```

<img width="1847" height="921" alt="image" src="https://github.com/user-attachments/assets/ad232497-44f9-40a1-abaa-d4943aba5a81" />


✅ Installation Complete! You can now explore the full RTL-to-GDSII flow using OpenROAD.

### `ORFS Directory Structure and File formats`

OpenROAD-flow-scripts/

```plaintext
├── OpenROAD-flow-scripts             
│   ├── docker           -> It has Docker based installation, run scripts and all saved here
│   ├── docs             -> Documentation for OpenROAD or its flow scripts.  
│   ├── flow             -> Files related to run RTL to GDS flow  
|   ├── jenkins          -> It contains the regression test designed for each build update
│   ├── tools            -> It contains all the required tools to run RTL to GDS flow
│   ├── etc              -> Has the dependency installer script and other things
│   ├── setup_env.sh     -> Its the source file to source all our OpenROAD rules to run the RTL to GDS flow
```
<img width="733" height="274" alt="image" src="https://github.com/user-attachments/assets/6bfbb4cc-4bf0-4975-aaf1-500317e6d25b" />

Inside the `flow/` Directory

```plaintext
├── flow           
│   ├── design           -> It has built-in examples from RTL to GDS flow across different technology nodes
│   ├── makefile         -> The automated flow runs through makefile setup
│   ├── platform         -> It has different technology note libraries, lef files, GDS etc 
|   ├── tutorials        
│   ├── util            
│   ├── scripts                 
```
<img width="736" height="212" alt="image" src="https://github.com/user-attachments/assets/87fc885a-bed0-42c6-a3c5-16f816febe98" />

# Floorplanning

This section describes OpenROAD-flow-scripts floorplanning and placement functions using the GUI

## Floorplan Initialization Based On Core And Die Area

Refer to the following OpenROAD built-in examples

```bash
# init_floorplan
source "helpers.tcl"
read_lef Nangate45/Nangate45.lef
read_liberty Nangate45/Nangate45_typ.lib
read_verilog reg1.v
link_design top
initialize_floorplan -die_area "0 0 1000 1000" \
  -core_area "100 100 900 900" \
  -site FreePDK45_38x28_10R_NP_162NW_34O

set def_file [make_result_file init_floorplan1.def]
write_def $def_file
diff_files init_floorplan1.defok $def_file
```
Run the following commands in the terminal in OpenROAD tool root directory to build and view the created floorplan.

```bash
cd ../tools/OpenROAD/src/ifp/test/
openroad -gui
```
<img width="737" height="481" alt="image" src="https://github.com/user-attachments/assets/2439da58-ba40-4a21-891a-a764d162043b" />

<img width="1498" height="666" alt="image" src="https://github.com/user-attachments/assets/dd97157b-3285-4f72-966f-f787fb1a989e" />


In Tcl Commands section GUI:
```bash
source init_floorplan1.tcl
```
View the resulting die area “0 0 1000 1000” and core area “100 100 900 900” in microns shown below:
<img width="1849" height="923" alt="image" src="https://github.com/user-attachments/assets/4dfbc7cf-8d50-4c56-b1e4-c5ccdc0c3453" />

## Floorplan Based On Core Utilization

Refer to the following OpenROAD built-in examples

```bash
# init_floorplan -core_space < site height
source "helpers.tcl"
read_lef Nangate45/Nangate45.lef
read_liberty Nangate45/Nangate45_typ.lib
read_verilog reg1.v
link_design top
initialize_floorplan -utilization 30 \
  -aspect_ratio 0.5 \
  -core_space 1 \
  -site FreePDK45_38x28_10R_NP_162NW_34O

set def_file [make_result_file init_floorplan2.def]
write_def $def_file
diff_files init_floorplan2.defok $def_file
```

Run the following commands in the terminal in OpenROAD tool root directory to view how the floorplan initialized:

```bash
cd ../tools/OpenROAD/src/ifp/test/
openroad -gui
```

In the Tcl Commands section of the GUI:
```bash
source init_floorplan2.tcl
```

View the resulting core utilization of 30 created following floorplan:

<img width="1843" height="930" alt="image" src="https://github.com/user-attachments/assets/5f899ab6-c5b0-4394-8f97-19e922c56353" />

## IO Pin Placement

Place pins on the boundary of the die on the track grid to minimize net wirelengths. Pin placement also creates a metal shape for each pin using min-area rules.

For designs with unplaced cells, the net wirelength is computed considering the center of the die area as the unplaced cells position.

Launch openroad GUI by running the following command(s) in the terminal in OpenROAD tool root directory:
```bash
cd ../tools/OpenROAD/src/ppl/test/
openroad -gui
```
<img width="1444" height="899" alt="image" src="https://github.com/user-attachments/assets/2d415616-cbb2-4fee-b84c-4c754166184e" />

Run place_pin4.tcl script to view pin placement.

```place_pin4.tcl
# gcd_nangate45 IO placement
source "helpers.tcl"
read_lef Nangate45/Nangate45.lef
read_def gcd.def

place_pin -pin_name clk -layer metal7 -location {40 30} -pin_size {1.6 2.5} -force_to_die_boundary
place_pin -pin_name resp_val -layer metal4 -location {12 50} -pin_size {2 2} -force_to_die_boundary
place_pin -pin_name req_msg[0] -layer metal10 -location {25 70} \
  -pin_size {4 4} -force_to_die_boundary

place_pins -hor_layers metal3 -ver_layers metal2 -corner_avoidance 0 -min_distance 0.12

set def_file [make_result_file place_pin4.def]

write_def $def_file

diff_file place_pin4.defok $def_file
```
From the GUI Tcl commands section:
```bash
source place_pin4.tcl
```
View the resulting pin placement in GUI:
<img width="1848" height="916" alt="image" src="https://github.com/user-attachments/assets/a53d5554-3135-49be-84a0-fe79b54708fc" />
<img width="1284" height="588" alt="image" src="https://github.com/user-attachments/assets/fc536e37-6be8-46f2-98c3-f10277fd59fb" />

# Macro or Standard Cell Placement

## Macro Placement

In this section, you will explore various placement options for macros and standard cells and study the impact on area and timing.

Refer to the following built-in example here to learn about macro placement.

```bash
source helpers.tcl
set test_name macro01
read_lef ./nangate45.lef
read_lef ./bp_be_top_macro.lef
read_def ./$test_name.def

global_placement -density 0.7
set def_file [make_result_file $test_name.def]
write_def $def_file
diff_file $def_file $test_name.defok
source report_hpwl.tcl
```
Placement density impacts how widely standard cells are placed in the core area. To view this in OpenROAD GUI run the following command(s) in the terminal in OpenROAD tool root directory:

```bash
cd ../tools/OpenROAD/src/gpl/test/
openroad -gui
```
<img width="869" height="894" alt="image" src="https://github.com/user-attachments/assets/b395a425-500c-4a12-a777-72b41d93e34e" />

In the Tcl Commands section of GUI

```
source macro01.tcl
```
Read the resulting macro placement with a complete core view:

<img width="1849" height="917" alt="image" src="https://github.com/user-attachments/assets/847762c9-1884-4f4e-9b6b-9cbf1eda1b98" />

<img width="999" height="681" alt="image" src="https://github.com/user-attachments/assets/f4e0b5bf-21a4-446c-b5da-4bda18916d36" />
