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

