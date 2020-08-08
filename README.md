
# vsdsram
### An SRAM IP Uniquely designed with open source tools.

Static RAM is a type of random-access memory that uses latching circuitry (flip-flop) to store each bit.

The Size of SRAM Specs is 32kbit/4k Bytes with 1.8v. 

To get insight of mem4kBytesOr32kbits design Specifications, check [this](/Documentation/mem4kBytesOr32kbitsSpec.pdf).

## Block Symbol and pins

![](Circuit-Inv/BlockSram.PNG)

### Circuit Diagram of SRAM and Its Read Write Analysis

![](Circuit-Inv/Sram.PNG) 

A 6T sram pairs up with two access transistors for read, write state and cross coupled inverter to hold/regenerate the state.

During **write operation** i.e., to write Q=0 while initial Q=vdd or 1, when the voltage at node Q reaches to a threshold voltage wherein PMOS M5 gets ON and the voltage at node Qbar starts to rise and the regenerative action of the cross-coupled inverter will force the write Q = 0. 
To write logic 0, Bitlline should be logic 0 and BLbar is complemented to 1. 
**Wordline is high**.

During **Read operation** i.e., to read Q=vdd or 1, when the voltage at node Qbar reaches to a threshold voltage wherein NMOS M3 gets ON and the voltage at node Q starts to fall and cross-coupled inverter state will force to flip the bit in a cell. 
**Wordline is high**.

### Cell Ratio and Pull-up Ratio Analysis

**Cell Ratio** is the ratio between the sizes of the driver transistor to access transistor.

From the circuit diagram reprsentation(transistor labels); For OSU180nm Technology.

CR = ((W3/L3)/(W2/L2))=((W4/L4)/(W6/L6))=((0.36u/0.18u)/(0.36u/0.18u))=1 

![](Waveforms/Ngspice/Spice/CellRatio.PNG) 

**Pull-up Ratio** is the ratio between load transistor to the access transistor. 

PR = ((W1/L1)/(W2/L2))=((W5/L5)/(W6/L6))=((0.9u/0.18u)/(0.36u/0.18u))=2.5

![](Waveforms/Ngspice/Spice/PullupRatio.PNG) 

### Static Noise Margin (SNM) Analysis

Static noise margin (SNM) is a functional metric to analyse the stability of SRAM cell. The graphical representation of SNM is through the largest square diagonal between the voltage transfer characteristics (VTC) of inverters. It determines the capability to retain the information that is stored. SNM measures the SRAM stability and it defines the maximum noise margin that a cell can store the data without losing the information.

![](Waveforms/Ngspice/SNM-Butterfly.PNG) 

## Upcoming Future Works

The Circuit design of 6T SRAM alongwith Precharge, Sense amplifier is designed solely in opensouce tool and its characterstics is analyzed. As a post layout stage, the design's layout is drawn and spice netlist is extracted. As a part of upcoming work the next step is post layout simulation, verifying it with pre layout. The32kbit and PnR (Place and Route) flow and lastly GDS to be released.

## IP USAGE

The user of this IP has to install `Ngspice` for Circuit simulation and `Magic` for Layout.

## Getting Started with NGspice: An open Source Circuit Simulator

*For ubuntu linux*

To Install Ngspice, Open terminal and Type 

`sudo apt-get install -y ngspice`

### To Clone the IP Repository

1. Download the Zip file or use `git clone https://github.com/Anushar123/vsdsram`

2. Change the path or directory using `cd vsdsram/Circuit-Inv` to view the circuit.

3. Change the path or directory using `cd vsdsram/Netlist` to run the simulation.

### To run PreLayout Simulations

1. Go to the path or directory using `cd vsdsram/Netlist`

3. *To simulate transient Analysis* type :`ngspice Sram.cir.out`

   Or

   Type `ngspice` in terminal

   ngspice 1 -> source Sram.cir.out

### To View Simulation Plots:

ngspice 1 -> plot q

![](Waveforms/Ngspice/Spice/Q.PNG)

ngspice 1 -> plot qbar

 ![](Waveforms/Ngspice/Spice/Qbar.PNG)

ngspice 1 -> plot q qbar

 ![](Waveforms/Ngspice/Spice/Q-Qbar.PNG)
 
 ngspice 1 -> plot bl

 ![](Waveforms/Ngspice/Spice/BL.PNG)
 
 ngspice 1 -> plot blbar

 ![](Waveforms/Ngspice/Spice/BLbar.PNG)
 
 ngspice 1 -> plot bl blbar

 ![](Waveforms/Ngspice/Spice/BL-Blbar.PNG)
 
 
*To simulate dc Analysis*
 
Type :`ngspice sramdc.cir.out`

ngspice 1 -> plot bl q

![](Waveforms/Ngspice/Spice/Butterfly.PNG)

 
To exit from Ngspice Shell type:

ngspice 1 ->  exit

## Getting Started with Magic Layout

Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Magic has remained popular with universities and small companies.

*For ubuntu linux*

Download Magic from this [link](http://opencircuitdesign.com/magic/)


```html
Open Terminal and type below commands. 
cd Downloads/
tar xzf magic-7.5.232.tgz
cd magic-7.5.232/
sudo apt-get install m4
sudo apt-get install tcl-dev
sudo apt-get install tk-dev
sudo apt-get install blt
sudo apt-get install freeglut3
sudo apt-get install libglut3
sudo apt-get install libgl1-mesa-dev
sudo apt-get install libglu1-mesa-dev
./configure
sudo apt-get install csh
sudo make
sudo make install
```
Or

Install Qflow & It will install all necessary open source tools including magic.

Download qflow from this [link](http://opencircuitdesign.com/qflow/)

```html
Open Terminal and type below commands. 
cd Downloads/
tar xzf qflow-1.4.83.tgz
sudo apt-get update
sudo apt-get install qflow
```


### To View Magic Layouts

1. Open the repository and change directory to `cd vsdsram/Layout`

2. To view the layout, type on terminal : `magic sram6.mag` or `magic -T SCN6M_SUBM.10.tech sram6.mag`

3. Similarly for precharge and senseamplifier: `magic precharge6.mag` & `magic senseamplifier6.mag`

3. To extract the spice netlist : Go to Tkcon window and type these commands.

`extract all`

`ext2spice rthresh 0 cthresh 0` **To include Parasitics in spice netlist** 

`ext2spice`

![](Circuit-Inv/Sram.PNG) 

![](Layout/Sram.PNG)

![](Layout/Sram-Width,height.PNG) 


![](Circuit-Inv/Precharge.PNG) 

![](Layout/Precharge.PNG)

![](Layout/Precharge-width,height.PNG) 


![](Circuit-Inv/SenseAmplifier.PNG) 

![](Layout/senseamplifier.PNG)

![](Layout/senseamplifier-width,height.PNG)


Note: IP is yet under process and yet to release as a final view. 

Incase of any difficulties with respect to IP Usage so far, contact administrator or report as github Issue.

## Author
Anusha R

## Acknowledgements
* Kunalghosh Director VSD Corp.Pvt.ltd
* Philipp Guhring Software Architect at LibreSilicon Association 
* Aishik Das Technical designer Calnestor knowledge solutions private limited Ambuja Neotia.
* Tim Edwards, Senior Vice President of Analog and Design at efabless corporation

# Contact Information
* Anusha.R Mtech (VLSI Design and Embedded Systems), Bangalore. anusha005r@gmail.com
* Kunalghosh Director VSD Corp.Pvt.ltd Kunalghosh@gmail.com
* PHILIPP GUHRING Software Architect at LibreSilicon Association pg@futureware.at
* Dr. GAURAV TRIVEDI Co-Principal Investigator,ECIT Academy and Associative Professor, EEE Department, IIT Guwahati trivedi@iitg.ac.in
