# AUX_CELL GENERATION

In Open FASoC Flow to generate a automated Analog design few auxilaury cells are required to be created which cannot me implemented with existing library cells (like Header and SLC in temp_sence_gen).
They have simple analog functionality usually associated with the structure of the auxiliary cell.“ALIGN” tool is used to .lef and .gds for auxiliary cells.

<img width="285" alt="auxcell" src="https://user-images.githubusercontent.com/110079729/206891799-6afd091c-3800-48fa-8305-c827ce351485.png">

## ALIGN Tool

ALIGN is an open source automatic layout generator for analog circuits jointly developed under the DARPA IDEA program by the University of Minnesota, Texas A&M University, and Intel Corporation.

### ALIGN Installation

1. Prerequisites

```
* gcc >= 6.1.0 (For C++14 support)
* python >= 3.7
```
2.Commands to install Align tool

```
export CC=/path/to/your/gcc
export CXX=/path/to/your/g++
git clone https://github.com/ALIGN-analoglayout/ALIGN-public
cd ALIGN-public

#Create a Python virtualenv
python -m venv general
source general/bin/activate
python -m pip install pip --upgrade

# Install ALIGN as a USER
pip install -v .

# Install ALIGN as a DEVELOPER
pip install -e .

pip install setuptools wheel pybind11 scikit-build cmake ninja
pip install -v -e .[test] --no-build-isolation
pip install -v --no-build-isolation -e . --no-deps --install-option='-DBUILD_TESTING=ON'

```

3. Making ALIGN Portable to Sky130 tehnology


Clone the following Repository inside ALIGN-public directory

```
git clone https://github.com/ALIGN-analoglayout/ALIGN-pdk-sky130
```

move `SKY130_PDK` folder to `../ALIGN-public/pdks`

4. Running ALIGN TOOL

```
python -m venv general
source general/bin/activate
```
5. Commands to run ALIGN (goto ALIGN-public directory)


```
mkdir work
cd work
```
6. General syntax to give inputs
```
schematic2layout.py <NETLIST_DIR> -p <PDK_DIR> -c
```
7. Running a EXAMPLE on Sky130pdk
```
schematic2layout.py ../ALIGN-pdk-sky130/examples/five_transistor_ota -p ../pdks/SKY130_PDK/
```
## PLL GENERATOR


A Phase locked loop(PLL) mainly consists of the following four blocks:-
1. Phase Detector(PD)
2. Charge Pump (CP)
3. Voltage Controlled Oscillator (VCO)
4. Frequency Divider (FD)


### Charge Pump (CP)
Charge Pump is used to convert the digital measure of the phase difference done in the Phase Detector into a analog control signal, which is used to control the Voltage Controlled Oscillator in the next stage.
In the construction of the Charge Pump we use a current steering model which makes it a Analog block. Here when the UP signal is high the current flows from Vdd to output which charges the load capacitance. When the DOWN signal is high the current flows from load capacitance to ground which is discharging of the current.


<img width="346" alt="charge_pump" src="https://user-images.githubusercontent.com/110079729/206891806-abf7df6a-fd82-4c69-96dd-79ff510f72c2.png">
