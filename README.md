
# DIGITAL_VSD_SOC_Design_and_Planning

This is a two weeks workshop organized by VSD in association with NASSCOM
## Physical Design
Physical design in VLSI involves transforming a circuit's high-level description into a detailed physical layout on silicon. This process includes several key steps:

Partitioning: Dividing the design into smaller, manageable sections.

Floorplanning: Determining the placement of these sections within the chip to optimize space and performance.

Placement: Precisely positioning individual components within each section.

Routing: Connecting the components with wiring while minimizing delays and interference.

Verification: Ensuring the layout meets design specifications and is manufacturable.

The goal is to create a layout that optimizes performance, area, power consumption, and manufacturability, ultimately resulting in a functional and efficient chip.

## Introduction
![Screenshot 2024-08-08 111702](https://github.com/user-attachments/assets/9f069ebb-adfb-4181-8f68-926b39896a95)
Above picture depicts the Journey that we will be following

![Screenshot 2024-08-08 121458](https://github.com/user-attachments/assets/cb6c8766-cc3a-4b97-8493-714c241e2917)
![Screenshot 2024-08-08 121748](https://github.com/user-attachments/assets/4408dffd-8587-44e9-abd5-1f87317049aa)
In the whole process we need to make sure that O1 = O2 = O3 = O4.Every step needs to be done carefully.



## WORKSHOP CONTENTS

DAY 1 : OPEN-SOURCE EDA, OPENLANE & SKY130 PDK.
- What is an RTL to GDSII flow?
- Insight into the QFN-48 Chip: Pads, Core, Die, and IP Components
- SOC DESIGN USING OPENLANE
- Understanding OPENLANE Directory Structure
- Design Preparation Step
- Review files after design prep and run synthesis
- Characterization of Synthesized Results

DAY 2 :  GOOD FLOORPLAN VS BAD FLOORPLAN & INTRODUCTION TO LIBRARY CELLS.
- Chip Floorplanning Considerations
- Library Binding and Placements
- Cell Design and Characterization Flows
- General Timing and Characterization Parameters
- Floorplanning and Placement

DAY 3 : DESIGN LIBRARY CELL USING MAGIC LAYOUT AND NGSPICE CHARACTERIZATION.
- LABS For CMOS Inverter ngspice Simulations
- Inception of Layout & CMOS Fabrication Process
- SKY130 Tech File Labs

DAY 4 : Pre-layout timing analysis and importance to good clock tress(i.e minimum skewness).
- Timing Modelling using delay tables
- Timing analysis with ideal clocks using openSTA
- Clock Tree Synthesis TritonCTS and Signal integrity
- Timing Analysis with real clocks using OpenSTA

DAY 5 : Final steps for RTL2GDS flow using TritonRoute and openSTA
- Routing and Design Rule Check (DRC)
- Power Distribution Networking and Routing
- TritonRoute features




# DAY 1 - Inception of open-source EDA OpenLANE and sky130 PDK

## How To Talk to Computers
![Screenshot (46)](https://github.com/user-attachments/assets/86c2fab4-2904-4b7c-8a68-ff1daae728ba)

The Arduino Uno board features a processor as the primary component of interest. The physical design will focus on the macro within the processor, implementing a block-level design.
The image below displays the microcontroller board or PCB with various components arranged on it.At the heart of the Arduino Uno is the microcontroller, typically an ATmega328P. The physical design of the microcontroller involves implementing a block-level design where various macros (such as the CPU core, memory blocks, and peripheral interfaces) are carefully arranged. Each macro is strategically placed to optimize the chip’s performance, power consumption, and manufacturability. This process involves several detailed steps, including floorplanning, placement, and routing, to ensure that all connections between macros are efficient and meet the required specifications.

![Screenshot (45)](https://github.com/user-attachments/assets/edd5523f-5b91-4793-b312-5a36bff1cca2)

## IC Design components and terminolgies

![Screenshot (37)](https://github.com/user-attachments/assets/2b31eba4-ccfe-4397-aece-9a10dcd2eec3)  ![Screenshot (38)](https://github.com/user-attachments/assets/ad9b3a63-0283-4b48-b4b8-902a8b44e191)

1. CORE : The core is the section of the chip where the fundamental logic of the design is placed.

2. DIE :The die is a segment of a chip that includes the core and I/O pads. To enhance production efficiency and yield, several copies of the die are patterned across the silicon wafer.

3. IO PADS: I/O pads are the pins that facilitate communication between the core and the external environment. These pads are positioned around the rectangular metal areas where external connections are made. They include input pads, output pads, and power pads.

4. IPs : Foundry IPs, such as SRAM, ADC, DAC, and PLLs, are typically designed with significant human involvement. They require manual design and engineering expertise to define and create. These IPs are essential components that integrate into a chip, and their design often involves customizing and optimizing them for specific applications and performance requirements.
 
5. PDKs : PDKs (Process Design Kits) serve as a bridge between foundries and design engineers. They provide essential files and models for IC design tools, covering aspects like device models, DRC, LVS, physical extraction, layers, LEF, standard cell libraries, and timing libraries. In this workshop, the SkyWater 130nm PDK, specifically sky130_fd_sc_hd, is used, and the openLANE toolchain is built around this PDK.

## Introduction to RISC-V
![Screenshot (234)](https://github.com/user-attachments/assets/7ab62a6e-85a1-4d2a-a0db-a6ef7d9847f5)
RISC-V Assembly Code (Left):
The process begins with a snippet of assembly code written for the RISC-V architecture. Each instruction corresponds to a fundamental operation that the CPU is built to execute. This code represents the lowest level of software, directly controlling the CPU's behavior.

Physical Layout (Right):
Finally, the HDL description is transformed into the physical layout of the picorv32 CPU core, visualized with tools like "qflow." Different colors and labels in the layout represent various circuit elements, including logic gates (e.g., AND, BUFX), flip-flops (e.g., DFFPOS), and interconnections. This layout dictates how the transistors and wires are arranged on the silicon chip to implement the CPU's functionality.

This visual representation encapsulates the intricate relationship between software and hardware in CPU design. Starting with high-level assembly code, the journey progresses through HDL design to the physical realization of the RISC-V core. This progression underscores how design decisions in the HDL directly impact the final silicon layout of the chip.

## Software to hardware
![Screenshot (236)](https://github.com/user-attachments/assets/a3c0c9e8-8627-4b2b-bef3-f06ab6a62c59)
![Screenshot (237)](https://github.com/user-attachments/assets/0b09684a-4bd9-4bf3-ac95-7172bce82abd)

**System Software and Compilation (Top):**
The Operating System (OS) serves as a bridge between software applications and hardware, managing system resources and providing an interface for applications to interact with the hardware. Compilers play a key role in this process by converting high-level programming languages (like C++) into assembly language instructions tailored to the specific CPU architecture.

**Assembly and Machine Code (Middle):**
Assemblers take the assembly code generated by compilers and translate it into binary machine code—comprising 0s and 1s—that the hardware can execute directly. At this level, the Instruction Set Architecture (ISA) acts as the interface between software and hardware, defining the specific set of instructions that the CPU can understand and perform.

**((Hardware Implementation (Bottom):**
Register Transfer Level (RTL): At this stage, the CPU's behavior is modeled using a Hardware Description Language (HDL) such as Verilog or VHDL. RTL descriptions capture the data flow between registers and the logic operations performed by the hardware.
Netlist Synthesis: The RTL code is synthesized into a netlist, which is a detailed blueprint of the circuit's components, including logic gates and flip-flops, and how they interconnect.

**Physical Design:**
In the final stage, the netlist is used to determine the physical placement and routing of transistors and wires on the silicon chip. This step produces the tangible hardware that will execute the software instructions originally written.

Key Insights:
This layered approach illustrates the complex journey from high-level software to physical hardware. It highlights the vital roles of compilers, assemblers, and hardware design tools in transforming abstract software instructions into real-world circuits. Understanding this flow is crucial for comprehending the interplay between software, hardware, and computer architecture.



 ## SIMPLIFIED RTL TO GDSII FLOW 
 The simplified RTL to GDS Flow is given in the image below
 
![Screenshot (241)](https://github.com/user-attachments/assets/f9a20374-6762-4824-86f6-fd969b5bffce)
 RTL Design: Create a high-level description of the circuit.

Synthesis: Convert RTL code to a gate-level netlist.

Floorplanning and Power Planning: Arrange blocks and plan power distribution.

Clock Tree Synthesis: Design the clock distributio
n network.

Routing: Connect the gates and flip-flops.

Signoff: Verify design with DRC, LVS, and ERC.

GDSII File Generation: Generate the final layout file for manufacturing.

 ## The OPENLANE Flow 
 
OpenLANE is an open-source EDA toolchain designed for digital ASIC design and optimization. It integrates various tools and flows to streamline the design process from RTL to GDSII. OpenLANE leverages the SkyWater 130nm PDK for standard cell libraries and design rules. It automates key steps such as synthesis, placement, and routing, enhancing design efficiency. The toolchain is tailored for academic and research purposes, facilitating open access to advanced ASIC design methodologies. 
 
![Screenshot (42)](https://github.com/user-attachments/assets/d1660bb9-5ba2-4f28-8134-419e8dad739a)
Overview of the OpenLANE Flow
The OpenLANE flow is a comprehensive process for digital chip design, covering everything from design exploration to physical implementation and final verification. The key stages of this flow include:

RTL Synthesis : This stage involves converting the Register Transfer Level (RTL) code into a gate-level netlist, representing the basic building blocks of the digital circuit.

Floorplanning & Placement : Here, the chip's layout is planned, and standard cells are strategically placed within the defined floorplan to optimize performance and area.

Clock Tree Synthesis (CTS) & Routing : In this stage, the clock tree is synthesized to ensure proper timing across the chip. Following this, the interconnections between the placed cells are routed using metal layers to establish the necessary electrical connections.

Static Timing Analysis (STA) : This step is crucial for verifying that the design meets all timing requirements, ensuring that signals propagate within the intended timeframes without causing errors.

Physical Verification : Physical verification includes Design Rule Checking (DRC) and Layout vs. Schematic (LVS) to confirm that the design adheres to manufacturing constraints and that the layout matches the original schematic.

Design for Test (DFT) : DFT techniques are applied, including scan insertion, automatic test pattern generation, test pattern compaction, fault coverage analysis, and fault simulation. These steps prepare the design for effective testing after fabrication.

Physical Implementation with OpenROAD : After DFT, physical implementation is carried out using the OpenROAD app, which further refines the layout.

Interconnect RC Extraction and STA : The routed layout undergoes RC extraction (DEF2SPEF) to model the resistive and capacitive properties of the interconnects. This data is then used in the OpenSTA tool (part of OpenROAD) to perform a detailed Static Timing Analysis, identifying any timing violations.

Physical Verification (DRC and LVS): Magic is used to check design rules (DRC) and extract SPICE models from the layout. Magic and Netgen are employed for Layout vs. Schematic (LVS) checks to ensure that the physical layout corresponds accurately to the original design.

Formal Verification : Since modifications to the netlist can occur during Clock Tree Synthesis (CTS) and post-placement optimizations, formal verification with Yosys's Logical Equivalence Check (LCE) is necessary. This step ensures that the functionality of the design remains consistent despite any changes made to the netlist during these stages.

This flow ensures that the design is robust, optimized, and ready for fabrication while maintaining the integrity of the original design specifications.








 # DAY 1 (LABS)
 ## Understanding the use of various linux commands :
 Open the LINUX Terminal (in desktop directory)
 
![Screenshot (65)](https://github.com/user-attachments/assets/7f4da44d-13ed-42a7-a444-890eb4604b13)


 Important LINUX Commands
1. cd : It is used to change the current working directory.
2. ls : It lists the contents of a directory.
3. ls -ltr : It lists the contents of a directory in long format, sorted by modification time in reverse order (oldest first).
4. ls --help: It displays a help message with a list of options and usage information for the "ls" command. Note : You can give any command name and then type "--help" to get info about that command.
5. cd : Using this command we can move in both ways in the directory tree.
6. clear: It clears the terminal screen.
7. pwd: Displays the current working directory path.
8. mkdir: Creates a new directory.
9. rm: Removes files or directories.
10. touch: Creates an empty file or updates the timestamp of an existing file.

 ### **Now, moving to the work directory where all files related to the workshop are stored**,use the follwoing commands
    cd Desktop 
    cd work/tools/ 
    cd openlane_working_dir/
    cd openlane/
    
   ![image](https://github.com/user-attachments/assets/4ec71fc5-d882-4fe5-b0ec-c79724a908a4)

  ### To enter into bash while being in the openalne dircetory use the  following command
  ~~~
      docker
      ./flow.tcl -interactive
  ~~~
      
   ![Screenshot (67)](https://github.com/user-attachments/assets/76f12cf7-421f-4260-8e0b-d7ab8e36e990)

      
   
###  Now, OPENLANE is opened, and we input the required packages using the following command:
     % package require openlane 0.9 
     
### In the 'designs' subdirectory, you will find various pre-built designs. For our purposes, we will focus on the "picorv32a.v" design to execute the RTL to GDS flow. The initial stage of this project involves synthesis. To begin the synthesis for this design, we need to set it up using the following command:
    prep -design picorv32a
   
   ![Screenshot (68)](https://github.com/user-attachments/assets/fbadc046-cb84-4a6e-a4a2-87716d25e083)

   Once the preparation is finished, you'll notice a new directory named with today's date created inside the 'picorv32a' folder within the 'runs' directory.

   ![Screenshot (69)](https://github.com/user-attachments/assets/e4addc8b-50a2-4ecb-b3c1-e71a71b9a50f)

###  A new directory structure is created to organize design files, with subdirectories for components like results and reports.


**LEF Merging : **
In this step, the Technology LEF (.tlef) and Cell LEF (.lef) files are merged into a single, unified format. This combined LEF file consolidates the layer definitions from the technology LEF with the cell information from the cell LEF, ensuring that all necessary data for physical design and layout is available in one file. This process simplifies the management and use of design rules and cell libraries during the chip design flow.
**Design Placement**: All design-related files are placed under the designs directory for organization and easy access during subsequent steps.

![Screenshot (70)](https://github.com/user-attachments/assets/7af31805-654a-4947-8f8b-86f568e0c7b0)

![Screenshot (71)](https://github.com/user-attachments/assets/d71596a2-325c-4d75-89da-a7fd86b81e69)

config.tcl contains all the design environment specifications

src -	contains verilog files and constraints file 

![Screenshot (72)](https://github.com/user-attachments/assets/3c480617-efc9-4f39-904f-6ab097ac5de0)

### Now, To perform synthesis on the design use the following command :
    % run_synthesis
    
![Screenshot (73)](https://github.com/user-attachments/assets/74635651-ca47-415e-bbb3-6a333fa6b19d)

After synthesis is done we will see the results of synthesis both in docker winodw and also in openlane directory under synthesis

 ### RESULTS

 formula to find out flop ratio is (number of D-ff/Total number of cells) 



![Screenshot (74)](https://github.com/user-attachments/assets/4d14b6c2-76bf-4fe0-aa50-a076571144ef)

![Screenshot (75)](https://github.com/user-attachments/assets/be1112f6-489a-4c67-a7c4-723837b9f418)

**Number of D-ff=1613 & Total number of cells=14876**

**therefore,Flop ratio=(1613/14876)=0.1084296853993**

**Flop ratio is %=10.84296853993%**

# DAY 2 Good floorplan vs bad floorplan and introduction to library cells
## Chip floor planning considerations
Utiliztion factor and aspect Ratio

In this section we will try to cover up the width and height of Core and Die. It is the first step in physical design flow to find out the width and height. Let's begin with a netlist, netlist is two flipflops and have a simple combination logic in between. A netlist describes the connectivity of an electronic design. Here, we dependent on the dimensions of the logic gates(AND & OR) and particular flipflop. Now, let's convert the symbols into physical dimensions. We are interested in the dimensions of the Core and Die not in the dimensions of the wires.

  To determine the utilization factor and aspect ratio, we first need to calculate the height and width of the core and die. 

![Screenshot (303)](https://github.com/user-attachments/assets/b69e5df7-04cd-4ea4-8f51-73f5090925c9)


 The dimensions of the core area are determined by the design's netlist, which depends on the number of components necessary to implement the logic. Consequently, the height and width of the die area are based on the dimensions of the core area. 

 For example, consider a netlist that includes two logic gates and two flip-flops.  

![Screenshot (78)](https://github.com/user-attachments/assets/502854c1-bc84-40d1-941c-980384185f32)

 Now, if we consider each element having an area of 1 square unit, and the netlist contains 4 elements, the minimum total area required for the core area will be 4 square units.

![Screenshot (79)](https://github.com/user-attachments/assets/2979870b-0016-4c3f-9b7d-d1aeaa63508e)

### Utilization Factor

The Utilization Factor is defined as the ratio of the core area occupied by the netlist to the total core area. In an effective floorplan, the Utilization Factor should not be 1. If the Utilization Factor reaches 1, there will be no available space for adding any additional logic, making it a poor floorplan.

![Screenshot (308)](https://github.com/user-attachments/assets/df09a327-9d0d-4ee7-8c46-8161b50141bc)


#### **Utilization Factor = (Area occupied by netlist / Total core area)**

### Aspect Ratio

The Aspect Ratio is defined as the ratio of the height of the core to its width. When the Aspect Ratio is 1, the core is considered square-shaped. If the Aspect Ratio is different from 1, the core will have a rectangular shape. 

 In the below case 

#### **Aspect Ratio = (Height of the core / Width of the core)**

![Screenshot (81)](https://github.com/user-attachments/assets/b869917d-fe89-4724-96de-cfcb96cff284)

#### **Utilisation Factor = (4 sq units)/(4sq units) = 1**
#### **Aspect Ratio = (2 units)/(2 units) = 1 i.e core has Square Shape**

 now if we take another case 

![Screenshot (82)](https://github.com/user-attachments/assets/065d20fc-3b52-460b-b664-e7c6b343f13d)

#### **Utilisation Factor = (4 sq units)/(8 sq units) = 0.5**
#### **Aspect Ratio = (2 units)/(4 units) = 0.5 i.e core has Rectangular Shape**

### PREPLACED CELLS : 
ets take a combinational logic which does some amount of function and assume its a huge circuit having some N Logic gates so let's devide it into some small numbers of gates. We will cut the whole circuit into two parts, and separate both of them into two blocks and both block will be implemented seperately.
![Screenshot (311)](https://github.com/user-attachments/assets/7990b49c-5ca7-485a-a999-406c6a96976a)
![Screenshot (312)](https://github.com/user-attachments/assets/1d845692-8137-4f22-bf33-ecb54e1e2c6b)

In both the blocks lets extend the input output pins and now we will black box the boxes and detached them. After black boxing, the upper portion is invisible from the top or invisible to the one , who is looking into the main netlist. now will seperate them out as two different IP's or modules.

Advantage of doing this is we can reuse them multiple times after implimenting once only. Similary there are other IP's also available for eg. Memory, Clock-gating cell, Comporator, MUX all of these are part of the top level netlist.They recieve some signals and perform functions and deliver the outputs but the functionality of the cell is implemented only once.


![Screenshot (314)](https://github.com/user-attachments/assets/d947303a-601c-475b-af11-879b82520b81)
The arrangement of these IP's in a chip is refferd as floorplanning.

These IP's have user-defined locations, and hence are placed in chip before automated placement and routing are called "pre-placed cells".

These cells are placed in such a way that, the placement and routing tool do not touch the location of the cell. DE-COUPLING CAPACITORS:Let consider same circuit, which is the part of the blocks which has been described earlier. When some gate (let consider AND gate) switched from 0 to 1 or 1 to 0, considered amount of the switching current required because of available small capacitance . This capacitor should be completely charged to represent logic 1 and completly discharged to represent logic 0. Consider capacitance to be 0. Rdd,Ldd,and Lss are well defined values. During switching operation, the circuit demands switching current i.e. peak current. Now, due to the presence of Rdd and Ldd, there will be a voltage drop across them and the voltage at Node 'A' would be Vdd' instead of Vdd.

![Screenshot (318)](https://github.com/user-attachments/assets/c1e976ff-8f01-4c1d-9991-2fcce4766b6b)

So, due to this if ideal logic 1 = 1 volt then here practically it can be less then 1 volt i.e., 0.97 volts (Vdd'). So, for any signal to be considered as Logic '0' and '1' in the NM low and NM high range. It is danger case.

![Screenshot (317)](https://github.com/user-attachments/assets/2e1c8e40-bb09-43f6-8ff6-a262319ed7b4)
To solve this problem,, we have to put De-coupling capacitor in parallel with the circuit. Every time the circuit switches, it draws current from Cd, whereas, the RL network is used to replenish the charge into Cd. And the amount of current needed for the circuit is supplied by the De- Coupling Capacitor.
![Screenshot (319)](https://github.com/user-attachments/assets/6ca7b9fb-ba7f-451f-b19a-a78a7b805298)
In the chip it will look something like shown below Decoupling capacitors are placed in between the block a, block b and block c. So here in this whole block it has been ensured that supply is being done by the de-coupling capacitor. Once we are done with this we have taken care of the local communication. Power planning:Now let's consider that local circuitory and keep it as a black box and it can be repeat multiple times and there is some logic present at the boundaries also and the problem of current demand was solved by de-coupling capacitor. There is signal which is send from driver to load and the signal is basically logic 0 to logic 1. Here we need to maintain the particular driver to load line with same signal so that the load recieves the same. Now power supply is applied. Now assume 16 bit bus has to retain the same signal from driver to the load. so it should get the sufficient power from the supply. But at this bus, there is no de-coupling capacitor is available because it is not physible to put capacitor at all over the place. now, power supply is far away from the bus, that is why some voltage drop between them will occur definetly.

![Screenshot (326)](https://github.com/user-attachments/assets/5c8f1bfb-e40c-4292-b0e5-4410894ae935)

The phenomenon we have seen was causing the lowering of the supply voltage,this problem occured because power has applied to one point only. The solution of the problem is use multiple power supply. So, every block will take charge from neartest power supply and similarly dump the charge to the nearer ground. this type of power supply is called mesh.

Final layout after powerplaning  is : 

![Screenshot (328)](https://github.com/user-attachments/assets/26df8c32-69ab-4b31-860f-ac1e4380a725)






# DAY 2 (LABS)
## STEPS TO RUN FLOORPLAN USING OPENLANE
To ensure a smooth floorplanning process, designers need to pay close attention to specific parameters, often referred to as switches, which can significantly influence the final floorplan when modified. Key examples of these switches include the utilization factor and the aspect ratio. It is essential for designers to confirm that these parameters meet the project requirements before beginning the floorplanning stage. The image below showcases various types of switches involved in this phase.


####  To run the Floorplan Use the following command:
     % run_floorplan
![Screenshot (83)](https://github.com/user-attachments/assets/faa0d020-84ed-4e17-a42e-de22b69e8de1)

![Screenshot (84)](https://github.com/user-attachments/assets/fa83f05b-05b3-41e7-8a21-b7c8d81208d7)

![Screenshot (85)](https://github.com/user-attachments/assets/b58de510-cd1b-48ba-8df8-1c97da8b1287)



 After completing the floorplan, you can review the generated report to evaluate aspects such as die area. To visualize the design in a graphical user interface (GUI), the MAGIC tool should be used. 

 #### Now, to open this "def" file in magic , use the following command:
      magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def 
      

![Screenshot (86)](https://github.com/user-attachments/assets/462fbfa2-37f4-4148-add7-7f2cb5333c26)

 ![Screenshot (87)](https://github.com/user-attachments/assets/79b26cf5-3b25-4340-81e3-926ff072e98a)
 
 Now, we can use 'Magic' tool to expllore the floorplan.
 
![Screenshot (88)](https://github.com/user-attachments/assets/26bf5c33-d69b-4924-a943-1334310f076b)

 THe DESIGN ALLIGNMENT INSTRUCTIONS ARE AS FOLLOWS: 

### Centering the Design:
    Press S to select the entire design.
    Press V to vertically align it to the middle of the screen.
### Zooming In on a Specific Area:   
    Left-click and drag to select the desired region.
    Right-click to bring up the context menu.
    Press Z to zoom in on the selected area.
### Getting Details of a Cell:
    Move your cursor to the cell of interest.
    Press S to select the cell.
### In the tkcon window, enter the command "what" to display cell details.

![Screenshot (89)](https://github.com/user-attachments/assets/60445c80-2a2a-4145-acca-b2b00a2fbc0d)

![Screenshot (90)](https://github.com/user-attachments/assets/11af4acb-eff2-4e02-b52e-9a27ebc66cb1)

## PERFORMING PLACEMENT IN OPENLANE
After successfully completing floorplanning, the design process progresses to the placement stage, which includes two main phases:

### Global Placement:
 During this phase, the tool determines the approximate locations for all the standard cells in the design.

### Detailed Placement:
  In this phase, the tool finalizes the exact positions for all the standard cells and ensures the placement is legal. Legalization involves verifying that no standard cells overlap and that they are all correctly positioned within the designated site rows.

### To initiate the placement process, use the following command
    run_placement
    
   ![Screenshot (91)](https://github.com/user-attachments/assets/82b5c708-b4d5-4897-a488-f86696a07ebe)

   ![Screenshot (92)](https://github.com/user-attachments/assets/b596055a-082d-4aae-b9c0-918d095b148c)

#### After the Placement is done , we can see 'picorv32a.placement.def' file , open it using MAGIC use the follwoing command:
     magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def

![Screenshot (93)](https://github.com/user-attachments/assets/d5e6d0bf-3d73-44f9-a443-ab973843577f)

![Screenshot (94)](https://github.com/user-attachments/assets/f750f9c0-aaa0-4864-a137-be506021b8e9)



## Cell design and characterization flows

Inputs for cell design flow

In Cell Design Flow, Gates, flipflops, buffers are named as 'Standard Cells'. These standard cells are being placed in the section called as 'Library'.And in the library many other cells are available which have same functionality but the size is different.

![Screenshot (382)](https://github.com/user-attachments/assets/d13de07e-f4d2-4f21-b660-f289966969c0)

![Screenshot (384)](https://github.com/user-attachments/assets/e25e7a95-585a-4ce3-96b0-f4e310e75c77)

If you look into one of the inverter from the library the cell design flowis as follows

The inverter has to represented in form of the shape, drive strength, power charracteristic and so on. Here cell design flow is devided into three parts.

Inputs

Design steps

Outputs

![celldesign2](https://github.com/user-attachments/assets/90dde6df-82a4-40cf-b71e-8d60ae9e723a)
### Circuit design Steps
![circuitdesign1](https://github.com/user-attachments/assets/badc98cc-6aa5-49a5-b1a4-f0ab80fcf010)

In circuit Design there are two steps.

First step is to implement the function itself and second step is to model the PMOS nad NMOS transistor in such a fashion in order to meet the libraray.

**Outputs**

The typical output what we get from the circuit design is CDL(circuit description language) file,GDSII,LEF,extracted spice netlist(.cir).
### Layout design steps


![layoutdesign](https://github.com/user-attachments/assets/91c7eb7a-7fa0-47d9-91da-8405b999fca2)
In Layout Design First step is to get the function implemented through the MOS transistor through a set of PMOS and NMOS transistor and the second step is to get the PMOS network graph and the nNMOS network graph out of the design that has been implemented.

![eulerpath](https://github.com/user-attachments/assets/f336c1a7-9bcc-47bd-85bf-e177302dd5c7)
After getting the network graphs next step is to obtain the Euler's path. Eule's path is basically the path which is traced only once.

![stickdiagram](https://github.com/user-attachments/assets/f525bd4d-f53f-4bb9-bfe0-24e19434f50f)
Next step is to draw stick diagram based on the Euler's path. This stick diagram is derived out of the circuit diagram.
![eulerpath to layout](https://github.com/user-attachments/assets/73680847-e784-47cb-b2d8-6ff7953d5ddf)

Next step is to convert this stick diagram into a typical Layout, into a proper layout and then get the proper rule we have discissed earlier. Once we get the particular layout then we have the cell width, cell length and all the specifications will be there like drain current, pin locations and so on.

Next and Final step is to extract the parasatics of that particular layout and charaterise it in terms of timing. So before that the output of the layout design will be GDSll. Once you get the extracted spice netlist then we characterize it. Characterization helps in getting timing, noise and power information.
### Typical characterization flow

Let's try to build the characterization flow based on the inputs we have,

First step is to read in the model, second step is to read the extracted spice netlist, third step is to define or recognize the behaviour of the buffer, fourth step is to read the subcircuits of the inverter and then in the fifth step need to attach the necessary power supplies, sixth step is to apply the stimulus then in the seventh step we need to provide the necessary output capacitance then in the final eighth step in which we need to provide necessary simulation command for example if we are doing transent simulation so we need to give .tran command , if we are doing DC simulation then we give .dc command.
Next step is to feed in all this inputs from 1 to 8 in a form of a configuration file to the characterization software "GUNA" .

![characterizationflow](https://github.com/user-attachments/assets/b460206e-09f7-4c52-a8e7-03c4cacb263b)
## D2 SK4 General timing characterization
![timing](https://github.com/user-attachments/assets/6be06b3a-4be8-4212-8465-b9b06fb7572f)
* **Slew Low Rise Threshold**: Start point of the rising edge transition.
* **Slew High Rise Threshold**: End point of the rising edge transition.
* **Slew Low Fall Threshold**: Start point of the falling edge transition.
* **Slew High Fall Threshold**: End point of the falling edge transition.
* **Input Rise Threshold**: Voltage level where input is considered 'high'.
* **Input Fall Threshold**: Voltage level where input is considered 'low'.
* **Output Rise Threshold**: Voltage level where output is considered 'high'.
* **Output Fall Threshold**: Voltage level where output is considered 'low'.

![propdelay](https://github.com/user-attachments/assets/e11d8058-d0e0-4978-aca3-af8f53fb924e)
**Propagation Delay**:

Time for a signal to propagate through a circuit element.
Measured at the 50% points of input and output transitions.
Affects circuit speed.




**Transition Time**:

Time for a signal to transition between voltage levels.
Rise time (tr): 10% to 90% transition.
Fall time (tf): 90% to 10% transition.
Affects signal integrity and performance.


  
 ![vtc](https://github.com/user-attachments/assets/a8d8aa39-b6b6-4e1a-9cb3-15534ccd6abd)
 ### Switching Threshold Vm


![static](https://github.com/user-attachments/assets/389775be-2fa6-4d41-b42f-613111b6bce3)

Switching thresold, Vm (the point at which the device switches the level) is the one of the parameter that defined the robustness of the Inverter. Switching thresold is a point at which Vin=Vout.






# DAY 3  Design library cells using magic layout and ngspice characterization

### CREATION OF SPICE DECK FOR CMOS INVERTER
 A Spice deck essentially contains the netlist with connectivity information, the inputs to be provided, output tap points, and additional details. 



![Screenshot (425)](https://github.com/user-attachments/assets/c259b339-24d1-441e-b20d-76709056d49a)

  Nodes are required to define the netlist 

![Screenshot (427)](https://github.com/user-attachments/assets/53df4293-e02c-40f6-8ed6-de9af91ba6f7)

Component Connectivity:
In this step, you need to define the connectivity of the substrate pin, which is crucial for tuning the threshold voltage of both PMOS and NMOS transistors. Proper connectivity ensures that the transistors operate with the desired electrical characteristics.

Component Values:
Here, you specify the values or dimensions of the PMOS and NMOS transistors. In this design, both PMOS and NMOS are given the same size, which likely implies symmetrical characteristics for the transistors in the circuit.

Identify the Nodes:
Nodes represent points in the circuit where components are connected. Identifying these nodes is essential for creating the circuit's netlist, which details the interconnections between all components.

Name the Nodes:
Once the nodes are identified, they are labeled with names such as Vin, Vss, Vdd, and Out, which help in referencing them throughout the design and simulation process.

Connections:

M1 MOSFET:

Drain: Connected to the Out node.
Gate: Connected to the In node (Vin).
Substrate and Source: Connected to the Vdd node (positive supply).
M2 MOSFET:

Drain: Connected to the Out node.
Gate: Connected to the In node (Vin).
Source and Substrate: Connected to ground (node 0).
CLOAD:
A load capacitor is connected between the Out node and ground (node 0) with a value of 10 femtofarads (ff).

Supply Voltage (Vdd):
Connected between the Vdd node and ground (node 0) with a value of 2.5V.

Input Voltage (Vin):
Connected between the Vin node and ground (node 0) with a value of 2.5V.

Simulation Commands:
In the simulation phase, the input voltage (Vin) is swept from 0 to 2.5V with a step size of 0.05V. The output voltage (Vout) is observed as Vin changes, allowing you to analyze the circuit's response.

Final Modeling:
The final step involves defining the model files, which provide a complete description of the NMOS and PMOS transistors, including their electrical characteristics and behavior within the circuit. This step ensures that the simulation accurately reflects the physical properties of the transistors.





**SPICE SIMULATION LAB FOR CMOS**

![Screenshot (446)](https://github.com/user-attachments/assets/45fc1fe0-54f3-47b0-94e0-f4a4fc549383)


## INVERTER CHARACTERISTICS
### STATIC CHARACTERISTICS

- **Switching Threshold (Vth)**: The voltage level at which the inverter switches from a high state (logic 1) to a low state (logic 0).
- **Input Low Voltage (Vil)**: The highest input voltage that is still interpreted as logic 0.
- **Input High Voltage (Vih)**: The lowest input voltage that is recognized as logic 1.
- **Output Low Voltage (Vol)**: The voltage level at which the output changes from a high state to a low state.
- **Output High Voltage (Voh)**: The voltage level at which the output changes from a low state to a high state.
- **Noise Margins**: The voltage ranges that define the tolerance for noise. The low noise margin is the range between Vil and Vol, while the high noise margin is the range between Vih and Voh.

### DYNAMIC CHARACTERISTICS

- **Propagation Delays**: The duration required for the output to respond after a change in the input.
- **Rise Time (tr)**: The time it takes for the output to move from the low voltage level (Vol) to the high voltage level (Voh).
- **Fall Time (tf)**: The time it takes for the output to move from the high voltage level (Voh) to the low voltage level (Vol).

  ## CMOS FABRICATION PROCESS
 ### Creating active reagion for transistors
![Screenshot (99)](https://github.com/user-attachments/assets/5f3f6044-7eaa-4f84-8e96-bde0493963d1)

  ### Formation of Nwell and Pwell
![Screenshot (100)](https://github.com/user-attachments/assets/3fff703e-0426-4c9f-b44f-c044a81d5a3d)

  ### Formation of 'gate' terminals
 ![Screenshot (101)](https://github.com/user-attachments/assets/d2450c55-6f64-4a55-a750-4b47b12e4c1f)

  ### Formation of LDD
 ![Screenshot (102)](https://github.com/user-attachments/assets/f86db4c6-ec68-43d6-b016-c8cb4477615f)

  ### Source and Drain formation
 ![Screenshot (103)](https://github.com/user-attachments/assets/a8d4f1a1-7f06-42b3-98e2-a0ee80df4103)

  ### Formation of contacts and interconnect
 ![Screenshot (104)](https://github.com/user-attachments/assets/d64a302d-e30d-4b85-a6a1-4431809cf5ea)

  ### higher level metal formation
 ![Screenshot (105)](https://github.com/user-attachments/assets/98e4c135-7f74-45bf-bdb9-4be24ffa44f4)

 # DAY 3 (Labs)
  First, we modified the IO PINS of the macro in the floorplan DEF file as shown in the image below. Then, we reran the floorplan and observed the changes. 
 set ::env(FP_IO_MODE) 2
 after making changes in floorplan def file that the pins are no more equidistant. 

![Screenshot (106)](https://github.com/user-attachments/assets/039be199-08f0-4a88-8b68-32b81bf74b47)

 ## GIT CLONE THE "vsdstdcelldesign"
#### Go to Openlane directory and use the following command:
    git clone <url of the github repo you want to clone>
    git clone https://github.com/nickson-jose/vsdstdcelldesign.git
![Screenshot (107)](https://github.com/user-attachments/assets/fa084127-8b0f-4870-838c-587dbcd36f1c)

 ![Screenshot (108)](https://github.com/user-attachments/assets/a01a873e-4723-4b57-b5f4-ffda9aa1bcac)
 ##### From the above image, we can see that the cloning was successful. Next, we will open the 'mag' file, which requires the 'tech' file located in the following directory:
       vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic$ 
 ##### so first we copy that file here, by using the following command:
       cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
 ##### It is copied successfully. Now, open the mag file magic
       magic -T sky130A.tech sky130_inv.mag &
  
 ![Screenshot (109)](https://github.com/user-attachments/assets/f9b8c54c-b388-45bb-a249-a80819ede81b)
 
  ![Screenshot (110)](https://github.com/user-attachments/assets/d4d6c544-69dc-4085-81f6-7a3f87176eae)

## TO EXTRACT THE NETLIST IN MAGIC
    % extract all
    % ext2spice cthresh 0 rthresh 0
 
![Screenshot (111)](https://github.com/user-attachments/assets/5bfedf57-7ec3-4c54-880a-ec6a2d5f17f0)

![Screenshot (112)](https://github.com/user-attachments/assets/ec669143-01d9-42da-95f4-96a35cd09fae)

 Now, lets open the created spice file:

![Screenshot (113)](https://github.com/user-attachments/assets/19abd143-750e-48d8-b612-2798c43adef1)

 Now the next step is to run the SPICE file in ngspice tool by using command ngspice sky130_inv.spice 

![Screenshot (114)](https://github.com/user-attachments/assets/0ea430ff-b34d-4ced-941e-ae8951f3687e)

#### Inverter Characterization using Sky130 Model Files
In this lab, we will characterize an inverter using ngspice and Sky130 model files, aiming to extract key parameters from the simulation results.

**Parameters to Characterize:**

1. **Rise Time:**
   - Definition: The time taken for the output waveform to transition from 20% to 80% of its maximum value.
   - Data Points: 
     - \( x_0 = 6.16138 \times 10^{-9} \) s, \( y_0 = 0.660007 \)
     - \( x_1 = 6.20366 \times 10^{-9} \) s, \( y_1 = 2.64009 \)
   - Calculation: 
     - Rise time \( = x_1 - x_0 = 0.0422 \) ns

2. **Fall Time:**
   - Definition: The time taken for the output waveform to transition from 80% to 20% of its maximum value.
   - Data Points: 
     - \( x_0 = 8.04034 \times 10^{-9} \) s, \( y_0 = 2.64003 \)
     - \( x_1 = 8.06818 \times 10^{-9} \) s, \( y_1 = 0.659993 \)
   - Calculation: 
     - Fall time \( = x_1 - x_0 = 0.0278 \) ns

3. **Propagation Delay:**
   - Definition: The time taken for a 50% transition at the output (0 to 1) corresponding to a 50% transition at the input (1 to 0).
   - Data Points: 
     - \( x_0 = 2.18449 \times 10^{-9} \) s, \( y_0 = 1.64994 \)
     - \( x_1 = 2.15 \times 10^{-9} \) s, \( y_1 = 1.65011 \)
   - Calculation: 
     - Propagation delay \( = x_1 - x_0 = 0.034 \) ns

4. **Cell Fall Delay:**
   - Definition: The time taken for a 50% transition at the output (1 to 0) corresponding to a 50% transition at the input (0 to 1).
   - Data Points: 
     - \( x_0 = 4.05432 \times 10^{-9} \) s, \( y_0 = 1.65 \)
     - \( x_1 = 4.05001 \times 10^{-9} \) s, \( y_1 = 1.65 \)
   - Calculation: 
     - Cell fall delay \( = x_1 - x_0 = 0.0043 \) ns
## INTRODUCTION TO SKY130 PDK
### Now us the follwoing command to download the Lab files while being in the home directory :
    sudo wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

![Screenshot (115)](https://github.com/user-attachments/assets/91f43ef9-04ba-4519-9b83-ba97fafe940c)

### Now that you have downloaded the zip file, use the following command to extract the lab files:
    sudo tar xfz drc_tests.tgz
 ![Screenshot (116)](https://github.com/user-attachments/assets/b8db3815-8c8f-41ed-a85c-5de1cf1bb402)

 In the downloaded files , .magicrc file serves as the start-up script for MAGIC 

### INTRODUCTION TO MAGIC & STEPS TO LOAD SKY130 TECH RULES
 Run the command magic -d XR to open the Magic tool.

 ![Screenshot (117)](https://github.com/user-attachments/assets/bd1fd2ad-0bfe-4cd8-8e8b-6ec2c656e0ed)
 Let us start by opening met3.mag file in magic
 
![Screenshot (118)](https://github.com/user-attachments/assets/87038c85-1e50-4844-a5ad-8612572b7155)

### Instructions:

#### Select an Area and Fill with Metal 3

1. **Open the Magic GUI.**
2. **Select the desired area on your layout.**
3. **Navigate to the metal 3 layer.**
4. **Press `P` to fill the selected region with metal 3.**

#### Create the VIA2 Mask

1. **Open the tkcon terminal within Magic.**
2. **Type the command:** `cif see VIA2`.
3. **The metal 3-filled area will now be associated with the VIA2 mask.**

![Screenshot (119)](https://github.com/user-attachments/assets/0d502558-4e84-46aa-99db-77fc432de2b5)

![Screenshot (120)](https://github.com/user-attachments/assets/5f1c0960-f5a2-4e74-bbb7-58f8f8cc585d)

use 'feed clear' to go back to previous view

## Lab exercise to fix Poly-9 error in Sky130 tech file
 **Fixing poly.9 error in Sky 130 tech-file**

open the plu.mag file

`gvim sky130A.tech
`

![2222](https://github.com/chmvs/vsd_openlane_workshop/assets/129481779/b404fc32-0bc2-4da0-8bf9-887e5e172027)

`change to allpolynonres`

![Screenshot 2024-06-02 000729](https://github.com/chmvs/vsd_openlane_workshop/assets/129481779/2f6455ae-8049-41e9-a987-9b2f1f31bdb4)

![Screenshot 2024-06-02 004438](https://github.com/chmvs/vsd_openlane_workshop/assets/129481779/aec02618-74cf-481d-8a57-a075a35c78b3)

implement poly resistor spacing to diffusion and tap

add this line---------------------
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching illegal \
    "xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"

![Screenshot 2024-06-02 000827](https://github.com/chmvs/vsd_openlane_workshop/assets/129481779/15c8a59f-d61e-4298-8b20-815afdff2aa3)

![Screenshot 2024-06-02 003108](https://github.com/chmvs/vsd_openlane_workshop/assets/129481779/1ddd77a5-14fb-433e-ac58-435901d72dd4)

**Describing DRC error as Geometrical Construct**

First we need to open `nwell.mag`

New commands inserted in sky130A.tech file to update drc
![Screenshot 2024-06-02 014141](https://github.com/chmvs/vsd_openlane_workshop/assets/129481779/1909b8bc-aa41-4628-9987-8b4a5c00a696)

**Commands to run in tkcon window**

**Loading updated tech file**
tech load sky130A.tech

**Change drc style to drc full**
drc style drc(full)

**Must re-run drc check to see updated drc errors**
drc check

**Selecting region displaying the new errors and getting the error messages** 
drc why

After implementing the rules mentioned above
![Screenshot 2024-06-02 014238](https://github.com/chmvs/vsd_openlane_workshop/assets/129481779/2062fa05-8e3c-4414-8618-a0c2dac9f0b9)

# DAY 4 Prelayout timing analysis and importance of good clock tree



**Timing Modeling Using Delay Tables**

Lab Task for Day 4 

1. Fix DRC Errors:
Address minor Design Rule Check (DRC) errors and ensure that the design is ready to be integrated into the flow.

2. Save and Open Final Layout:
Save the finalized layout with a custom name and open it to review.

3. Generate LEF from Layout:
Create a LEF file from the finalized layout.

4. Copy LEF and Associated Files:
Move the newly generated LEF file along with the necessary library files to the picorv32a design src directory.

5. Edit config.tcl:
Update the config.tcl file to reference the new library files and add the newly generated LEF to the OpenLANE flow.

6. Run Synthesis with Custom Inverter Cell:
Execute the OpenLANE flow synthesis, incorporating the newly added custom inverter cell.

7. Resolve Violations:
Address and minimize any new violations introduced by the custom inverter cell by adjusting design parameters.

8. Verify in PnR Flow:
Once the synthesis has accepted the custom inverter, proceed with floorplanning and placement to ensure the cell is correctly integrated into the Place and Route (PnR) flow.

9. Post-Synthesis Timing Analysis:
Perform timing analysis using the OpenSTA tool to ensure the design meets timing requirements.

10. Timing ECO Fixes:
Make Engineering Change Order (ECO) adjustments to resolve any timing violations.

11. Update Netlist:
Replace the old netlist with the new one generated after applying timing ECO fixes, then reimplement the floorplan, placement, and Clock Tree Synthesis (CTS).

12. Post-CTS Timing Analysis:
Conduct timing analysis after CTS using the OpenROAD tool.

13. Explore Timing Analysis:
Investigate the impact of removing the sky130_fd_sc_hd__clkbuf_1 cell from the clock buffer list (CTS_CLK_BUFFER_LIST) during post-CTS timing analysis with OpenROAD.

LEF and DEF Files
LEF (Library Exchange Format):
LEF files define the physical properties of standard cells, macros, and other library components. While they don’t specify cell placement, they provide crucial details for placing and routing the design.

DEF (Design Exchange Format):
DEF files describe the actual placement and routing of cells in the design, including the precise position of each cell and the routing of interconnections.

**Conditions to Satisfy Before Creating a Standard Cell**

The input and output ports of the standard cell must align with the intersections of vertical and horizontal tracks.
The width of the standard cell should be an odd multiple of the horizontal track pitch.
The height of the standard cell should be an even multiple of the vertical track pitch.
Track File Reference:
Refer to the track file located at pdk/sky130/libs.tech/openlane/sky130_fd_sc_hd/track.info for detailed information on track pitch and alignment.


Open the track file from```pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info```


![2](https://github.com/user-attachments/assets/a070a0e9-19f6-477c-b2f4-b38e3e3a9a92)  

**Commands to open custom inverter layout**

Change directory to vsdstdcelldesign : 
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

Command to open custom inverter layout in magic : 
magic -T sky130A.tech sky130_inv.mag &

Get syntax for grid command :
help grid

Set grid values accordingly : 
grid 0.46um 0.34um 0.23um 0.17

![4](https://github.com/user-attachments/assets/faf4a323-88bc-4e98-b650-9eb241d3e1dc)

![5](https://github.com/user-attachments/assets/94f852b7-46dd-407d-9d6f-9c990baf5a4e)

![6](https://github.com/user-attachments/assets/fe205bc2-2bc6-4318-b117-9f23493e69c6)

**Opening newly created inverter and check various port names and its value**

Command to save as : 
save sky130_vsdinv.mag

Command to open custom inverter layout in magic : 
magic -T sky130A.tech sky130_vsdinv.mag &


![7](https://github.com/user-attachments/assets/b3d15096-16aa-498e-91c4-f7445bf1f493)



**Generating lef from layout**  
```bash
# lef command
lef write
```

![10](https://github.com/user-attachments/assets/f3b6f92a-2a3d-46c2-ba74-b045e7441b8d)  



![11](https://github.com/user-attachments/assets/74d237fc-fc4f-4793-909c-db212fd91c6f)
![12](https://github.com/user-attachments/assets/0774ef85-57b2-4646-86f7-2f9c65ec232d)


![14](https://github.com/user-attachments/assets/106cf674-4eae-4182-8f76-8d279dbb7b42)  
**Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow**  
**Commands to be added in config.tcl**

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]


![15](https://github.com/user-attachments/assets/ebcd5ed2-c497-4982-b44b-dae58b304dca)
![16](https://github.com/user-attachments/assets/ec41bbf1-e270-4905-bbee-9545654ab3f1)

**Run openlane flow synthesis with newly inserted custom inverter cell**

Here’s the revised text broken down into points:

1. **Navigate to the OpenLANE Directory:**
   - Use the command `cd Desktop/work/tools/openlane_working_dir/openlane` to change to the OpenLANE flow directory.
   - Enter the Docker environment by running `docker`.

2. **Launch OpenLANE in Interactive Mode:**
   - Once inside the OpenLANE Docker sub-system, initiate the OpenLANE flow in Interactive mode with the command `./flow.tcl -interactive`.

3. **Load Necessary Packages:**
   - After the OpenLANE flow is open, load the required packages for proper functionality by executing `package require openlane 0.9`.

4. **Prepare the Design:**
   - Create the necessary files and directories for running a specific design (in this case, `picorv32a`) using the command `prep -design picorv32a`.

5. **Include Newly Added LEF Files:**
   - To incorporate any newly added LEF files into the OpenLANE flow, use the following commands:
     - `set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`
     - `add_lefs -src $lefs`

6. **Run Synthesis:**
   - With the design prepped and ready, initiate the synthesis process by running `run_synthesis`.



![VirtualBox_vsdworkshop_08_08_2024_13_34_23](https://github.com/user-attachments/assets/af082b78-cf82-4f36-a7a8-85e01b779553)





Here we see even after changing command the slack that is wns(worst negative slack)=-23.89 and tns(totalnegative slack)=-711.59 has not reduced.But to reduce it we can use command  


```set ::env(SYNTH_STRATEGY) "DELAY 3"```

 which is more agressive to reduce wns and tns.Alltough ultimately slack will reduce after cts and routing is done even though it is not reduced now.


![VirtualBox_vsdworkshop_08_08_2024_20_37_11](https://github.com/user-attachments/assets/54b9dc6c-8a79-428d-b3fd-54eb3df5a4d8)


![VirtualBox_vsdworkshop_08_08_2024_20_40_51](https://github.com/user-attachments/assets/cc3bfea8-c429-4a24-80af-613a72140d19)

![VirtualBox_vsdworkshop_08_08_2024_20_41_18](https://github.com/user-attachments/assets/abfb423f-7e87-4889-8a95-1403136933a0)
**We see that tns and wns has reduced but correspondingly chip area has increased.So this is a trade off.Previous chip area was ```147712.918400```**


**However we use the ```set ::env(SYNTH_STRATEGY) 1 ```to run floorplan and placement and later reduce tns and wns**

![VirtualBox_vsdworkshop_04_08_2024_01_00_55](https://github.com/user-attachments/assets/49271fc5-33fb-4057-a15f-5f9e184e64cc)
![VirtualBox_vsdworkshop_03_08_2024_20_03_46](https://github.com/user-attachments/assets/1577a759-e742-4e05-a759-016b59b5c768)

![VirtualBox_vsdworkshop_03_08_2024_20_03_59](https://github.com/user-attachments/assets/c1e14930-e893-41e7-b778-81f77886d1f6)
![VirtualBox_vsdworkshop_03_08_2024_20_04_36](https://github.com/user-attachments/assets/85857e2c-41f4-4421-ba01-28f6881e7e73)


![VirtualBox_vsdworkshop_04_08_2024_01_07_20](https://github.com/user-attachments/assets/69497bc4-1fb1-4345-a9af-f3f81fc2488e)

![VirtualBox_vsdworkshop_04_08_2024_01_09_37](https://github.com/user-attachments/assets/d2227de2-23a2-49bb-9b76-1c19be6f54db)
**COMMAND TO OPEN MAGIC IN ANOTHER TERMINAL**
```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-07_10-25/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![VirtualBox_vsdworkshop_04_08_2024_01_13_06](https://github.com/user-attachments/assets/58da9098-5e8a-4603-8669-2527461e3d21)


![VirtualBox_vsdworkshop_04_08_2024_01_16_03](https://github.com/user-attachments/assets/66cdc14c-e40b-47f6-9fe1-86da8e2b35dc)

## D4 SK2 Timing analysis using ideal clock using openSTA

**Setup timing Analysis and Introduction to Flipflop setup time**



![setup](https://github.com/user-attachments/assets/b4045b45-7d4f-4460-8aed-3ed143dd3736)
Let's start the setup analysis with the ideal clock(single clock). specifications of the clock is

clock frequency =1 GHz

clock period =1 ns

Now will do the analysis between '0' and 'T' clock period. We sent at edge to the launch flop at '0' clock period and at T=1ns period the second edge reached to capture flop.

Let's say here we have combinatonal delay of theta and set up timing analysis says that this combinational delay should be less than the T for system to work properly.
Now let's open the capture flop and we will see some combinational circuit there it has several MOSFETs , several logics,resistances and capacitances inside it.Also have the time graph for this particular flop
When there is logic '0' or logic '1' of clock 1 the delay of MUX1 and MUX2 will restrict or effect the combinational delay requirement.

So there is some finite amount of time which is required to the D input to settle and this amount of time is reffered to as SET UP TIME.

Hence finite time 's' required before clk edge for 'D' to reach Qm.

So, we can write that the internal delay of the MUX1 = set up time(S).

So, now θ<T becomes θ<(T-S).
**Introduction to clock jitter and uncertainity**


![jitter1](https://github.com/user-attachments/assets/1081967d-3259-4ecf-a7b3-cdf41b2fa0e4)

So in Jitter the clock is being created by PLL(phase-locked loops) and the clk source is expected to sent the clk signal at exactly 0,T,2T,....But that clk source might or might ot be able to generate the clk exactly at 0 or any other certain time because of it's inbuilt variations that is called jitter. Jitter is refered as temporary variation of the clk pulse.
Let's consider this uncertantity time(US) in consideration. So, now equation will become θ<(T-S-US). Now assuming 'S'=0.01ns and 'US'=0.09ns. by taking this, Let's identify the timing path in our circuit stage 1 and stage 3 logic path has single clock.

Now,we have to identify the combinational path delay for the both logics.
**Do post synthesis timing analysis with opensta**
```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

  ![VirtualBox_vsdworkshop_04_08_2024_15_06_14](https://github.com/user-attachments/assets/50663432-cef4-4300-a8ee-c28ee42e5162)
  Newly created ```pre_sta.conf ```for STA analysis in openlane directory
  

![VirtualBox_vsdworkshop_04_08_2024_15_07_03](https://github.com/user-attachments/assets/6aec13fa-f00f-49b6-9fb9-f68afc4a8c0d)

Newly created ```my_base.sdc```for STA analysis in ```openlane/designs/picorv32a/src directory``` based on the file```openlane/scripts/base.sdc```

![VirtualBox_vsdworkshop_04_08_2024_15_11_46](https://github.com/user-attachments/assets/72263eb3-43e2-44a5-9747-1d180f724cc8)

![VirtualBox_vsdworkshop_08_08_2024_16_10_37](https://github.com/user-attachments/assets/6fb2f8e9-a152-4bcd-8354-129d5c117098)
```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```


![VirtualBox_vsdworkshop_08_08_2024_16_15_50](https://github.com/user-attachments/assets/93c3c46b-b3fb-463e-b421-1a174ef0ba10)

![VirtualBox_vsdworkshop_04_08_2024_15_11_22](https://github.com/user-attachments/assets/7749895f-e700-4d4f-83d2-c1377e85657e)
Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis
```bash
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 29-07_10-25 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

![VirtualBox_vsdworkshop_04_08_2024_15_12_54](https://github.com/user-attachments/assets/55183310-2ec8-4f2c-b3af-d75dba8edc95)

![VirtualBox_vsdworkshop_08_08_2024_18_46_36](https://github.com/user-attachments/assets/c9301065-ccb0-45fd-86b2-f42921c746bd)

![VirtualBox_vsdworkshop_04_08_2024_15_13_56](https://github.com/user-attachments/assets/36e9abc5-2168-4c03-b75c-b9df95fec940)
![VirtualBox_vsdworkshop_04_08_2024_15_16_30](https://github.com/user-attachments/assets/b8f5d6c5-2646-4264-8a94-da19ace94b88)
**Make timing ECO FIXES TO REMOVE ALL VIOLATION**
OR gate of driving strength 2 is driving 4 fanouts



![1](https://github.com/user-attachments/assets/57f5159e-03da-43f6-99d6-fc8b26f6e7d9)
**Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**
```bash
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

![2](https://github.com/user-attachments/assets/69de151b-f4a7-40e0-914e-f50c8a1151b5)


![3](https://github.com/user-attachments/assets/642862d9-722f-4c17-b264-354a9e5e1f87)

![4](https://github.com/user-attachments/assets/61aaa85a-5883-4ec1-af2e-f73c2797f861)
OR gate of drive strength 2 is driving 4 fanouts


![5](https://github.com/user-attachments/assets/94f2606a-591e-4ac6-bddd-5215654beb6f)
**Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**
```bash
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

![6](https://github.com/user-attachments/assets/5704beb9-03e3-4010-a19c-12c393c3a680)


![7](https://github.com/user-attachments/assets/7a672c62-1d6e-430f-bd52-71bd23b48daa)
OR gate of drive strength 2 driving OA gate has more delay

![8](https://github.com/user-attachments/assets/df0eb30b-f35a-471a-b364-14c89b71f024)
 **Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**
 ```bash
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

![9](https://github.com/user-attachments/assets/9897b255-c413-4f5d-b247-7dd5a39c1e1c)

![10](https://github.com/user-attachments/assets/d08759d7-b55d-4f37-951e-911a56a40016)

OR gate of drive strength 2 driving OA gate has more delay


![11](https://github.com/user-attachments/assets/4d749d2c-59ac-4524-ad53-df92ea2d8adc)
**Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4**
```bash

# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

![12](https://github.com/user-attachments/assets/9588846a-47ce-44a0-8289-1e023e2f2b0a)
![13](https://github.com/user-attachments/assets/465304ba-12c7-4678-b92b-8eb9f87a8d48)
**We see that the overall slack is reduced**
## D4 SK3 Clock Tree Synthesis TritonCTS and signal integrity
**Clock Tree routing and buffering Using H-tree algorithm**


![clock tree](https://github.com/user-attachments/assets/c30ea7b7-d33f-464e-a5d7-a8ef0e49413d)
**Clock tree synthesis**:Let's connect clk1 to FF1 & FF2 of stage 1 and FF1 of stage 3 and FF2 of stage 4 with physical wire.Now let's see what is the problem with this? Let's consider some physical distance from clk to FF1 and FF2 , so due to this t2>t1.

Skew= t2-t1, and skew should be 0ps.Previously we have build bad tree now we will try to modify that in a smarter way. Hrre clk will come in somewhere mid points with this clk will reach to every flip flop at almost same time. In the same way we will connect the clk2 with flip flops like midpoint manner.Now will see clock tree synthesis(Buffering), Let's we have some clock route through which it has to reach to particular locations and clock end points and in the path many capacitance, resitors are there.


![clock tree 1](https://github.com/user-attachments/assets/73ceea05-5fd3-4e9e-9e96-d7d33a6266fe)
Because of the wire length we did not get the same wave form at ouput as input and bcz of RC networks , so to resolve this problem we use repeaters. The only difference between the repeaters we use for clock or for data path is that clock repeaters repeaters will have equal rise and fall time.

First step is we will remove the clock route and place 2 repeaters and allow the clock to go through this particular repeater, in this case whatever wave form is generated here will go to the output. So we can as many as repeaters we want to make the continuous flow of th clock till the output.

![clocktree2](https://github.com/user-attachments/assets/30c0ac94-fe64-4d64-9057-b3b158c5258d)
#### Cross talk and Clock Net Shielding
**Clocknet shielding**:Is a technique used in digital integrated circuit (IC) design to minimize the effects of noise and crosstalk on the clock signal. The clock signal is one of the most critical signals in a chip, and its integrity is vital for the proper functioning of the circuit. Shielding helps ensure that the clock signal remains clean and consistent by reducing interference from neighboring signals.
**Crosstalk**:Crosstalk refers to the unwanted interference caused by a signal in one circuit or wire influencing a signal in an adjacent circuit or wire. In the context of integrated circuits (ICs), crosstalk primarily occurs when a signal in one net induces noise in a neighboring net due to capacitive or inductive coupling.

![cross talk](https://github.com/user-attachments/assets/850ab9af-eb9a-49a7-8369-d1b8a41c4793)

![cosstalk1](https://github.com/user-attachments/assets/e37e64d0-e734-4d17-a964-d0f04d9ba837)
**Lab steps to run CTS using triton**
**Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts**
```bash
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-07_10-25/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

![VirtualBox_vsdworkshop_08_08_2024_18_59_41](https://github.com/user-attachments/assets/3b97bf7f-15d8-410f-993e-8a29d85cbe30)
Command to write verilog
```bash

# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-07_10-25/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

![VirtualBox_vsdworkshop_04_08_2024_16_28_17](https://github.com/user-attachments/assets/68bb08ec-2ef1-4bdf-9686-5e628d205caf)
Verified that the netlist is overwritten by checking that instance``` _14506_``` is replaced with ```sky130_fd_sc_hd__or4_4```

![VirtualBox_vsdworkshop_04_08_2024_16_29_38-replacement successful](https://github.com/user-attachments/assets/a495367e-a364-451b-87ac-d97509b64d80)
Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages
```bash
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 29-07_10-25 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts
```
![VirtualBox_vsdworkshop_04_08_2024_17_46_04](https://github.com/user-attachments/assets/b5932cd2-25a4-4d19-8159-da6fcc69db88)

![VirtualBox_vsdworkshop_04_08_2024_17_46_47](https://github.com/user-attachments/assets/533cf13c-a690-46c7-bfd7-ad6bbadbbfed)

![VirtualBox_vsdworkshop_04_08_2024_18_10_31](https://github.com/user-attachments/assets/e0875ad1-4e6b-4a08-85e8-9c082ce202cf)
![VirtualBox_vsdworkshop_04_08_2024_18_11_46](https://github.com/user-attachments/assets/e78b1b35-b6bf-42e4-a1bf-3bf154f97c22)
## Timing analysis with real clocks using openSTA
**Setup analysis with real clock**


![setup with real](https://github.com/user-attachments/assets/8160aead-5e62-4151-8324-d643b67fd7c3)
**Hold analysis with real clock**



![hold analysis](https://github.com/user-attachments/assets/07c222b4-64a0-4a0a-a9d7-051f8ee69977)
![hold real](https://github.com/user-attachments/assets/ebbc6d54-2f9d-4fb7-b5fc-2fc587572c88)
**Post cts OPENROAD TIMING analyis**
```bash
Command to run OpenROAD tool
openroad

Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/29-07_10-25/tmp/merged.lef

Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/29-07_10-25/results/cts/picorv32a.cts.def

Creating an OpenROAD database to work with
write_db pico_cts.db

Loading the created database in OpenROAD
read_db pico_cts.db

Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/29-07_10-25/results/synthesis/picorv32a.synthesis_cts.v

Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

Link design and library
link_design picorv32a

Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

Check syntax of 'report_checks' command
help report_checks

Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

Exit to OpenLANE flow
exit
```


![VirtualBox_vsdworkshop_04_08_2024_19_51_05](https://github.com/user-attachments/assets/478def47-e87c-4087-ae99-35d71cd699fc)

![VirtualBox_vsdworkshop_04_08_2024_19_56_48](https://github.com/user-attachments/assets/8272d640-0f3c-43a0-a136-0cac50ce8c72)



![VirtualBox_vsdworkshop_04_08_2024_20_02_42](https://github.com/user-attachments/assets/e9366bdf-f754-4106-b532-e81d79bae1d3)


![VirtualBox_vsdworkshop_04_08_2024_20_03_41](https://github.com/user-attachments/assets/d2590de4-ffa5-4f0c-a8fb-e89f5c665e54)
**Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'**
```bash
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/29-07_10-25/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/29-07_10-25/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/29-07_10-25/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/29-07_10-25/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```
![VirtualBox_vsdworkshop_04_08_2024_23_07_15](https://github.com/user-attachments/assets/52ca802f-3cb4-45fc-a8bb-0eaea80fbbd8)

![VirtualBox_vsdworkshop_04_08_2024_23_16_20](https://github.com/user-attachments/assets/420254f6-d525-4ce4-bdbd-645bba916eca)

![VirtualBox_vsdworkshop_04_08_2024_23_24_31](https://github.com/user-attachments/assets/fc0e229d-118f-4ea0-90d0-7a72792b3640)

![VirtualBox_vsdworkshop_04_08_2024_23_31_23](https://github.com/user-attachments/assets/fa078dc7-1722-4453-b6bb-7a6bb95b6134)

![VirtualBox_vsdworkshop_04_08_2024_23_34_00](https://github.com/user-attachments/assets/486ae600-2a7c-449d-aec4-fa53c0d192d8)



# DAY 5  Final steps for RTL2GDS using Tritronroute and openSTA
##### To know which stage was done previously in the flow? use the follwoing command 
      echo $::env(CURRENT_DEF)
      
  ![Screenshot 2024-08-06 ](https://github.com/user-attachments/assets/63f0afd3-f4f7-4ffe-a46f-f3993508e58b)


 ## INRODUCTION TO ROUTING ALGORITHM
  #### maze routing
  ![Screenshot (170)](https://github.com/user-attachments/assets/142b01bc-d40b-4b55-b9fb-f2558b88ac91)
  
  ![Screenshot (171)](https://github.com/user-attachments/assets/3f49fe61-342f-4ef9-81b6-56950ee42180)

  #### Building routing grid with standard dimensions
  
  ![Screenshot (172)](https://github.com/user-attachments/assets/ee6a0cf3-6e44-4009-99c5-eebee8215426)

  There are various routes possible, but routes with fewer bands are preferred, ideally a single band. There are other algorithms that can replace it since it is more time-consuming. 
 
 ![Screenshot (173)](https://github.com/user-attachments/assets/034152d0-61ec-4794-b601-3ec5ebef1df9)

 While doing routing , DRC Rules need to be followe

## ROUTING
![Screenshot (174)](https://github.com/user-attachments/assets/c5780c36-2dca-4589-9781-0941ef5e1bef)

**Routing**: This process creates physical connections between VLSI design components after placement. It includes:

- **Global Routing**: Determines general paths on a grid without detailed specifics.
- **Detailed Routing**: Defines exact wire paths, geometries, and layers at the transistor level.

**Routing Algorithms**:
- **Maze Routing**: Finds optimal paths by exploring all possible routes on a grid.
- **Channel Routing**: Routes within constrained regions or channels.

**Routing Layers**: Use multiple metal layers, with lower layers for local and higher layers for global connections.

**Routing Challenges**:
- **Congestion**: Avoiding crowded paths to prevent performance issues.
- **Crosstalk**: Minimizing interference between adjacent signal paths.
- **Delay**: Managing signal propagation delays to meet timing constraints.

**Design Rule Checking (DRC)**: Ensures VLSI design adheres to manufacturing rules provided by the foundry. It involves:

- **Design Rules**:
  - **Minimum Width/Spacing**: Ensuring features meet size and distance requirements.
  - **Enclosure/Overlap**: Ensuring proper feature connectivity and functionality.
- **DRC Checks**:
  - **Geometric Checks**: Verify physical dimensions and spacing.
  - **Connectivity Checks**: Ensure correct connections and no shorts.
  - **Electrical Checks**: Verify electrical performance criteria.

**DRC Workflow**: Define rules, verify layout with DRC tools, correct errors, and re-verify.

# DAY 5(LABS)

##### Now the last stage in the design is Routing , us ethe following command:
      run_routing
 ![Screenshot (175)](https://github.com/user-attachments/assets/02b0bfd0-2e24-4ac0-867f-e64d0a32f33f)

![Screenshot (176)](https://github.com/user-attachments/assets/e05d1cf0-4333-4815-93c2-79044db63ab5)
![Screenshot (177)](https://github.com/user-attachments/assets/96d567bb-568c-462b-9dcd-7f4f6b564f94)

![Screenshot (178)](https://github.com/user-attachments/assets/64cf7d57-3439-4ff7-9ee4-8209f24a2cdc)

##### To view the final layout, use the following command:
       magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/22-07_15-00/tmp/merged.lef def read /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/22-07_15-00/results/routing/picorv32a.def &
       
  ![Screenshot (179)](https://github.com/user-attachments/assets/8a84c185-4bac-4252-8158-79891103983a)
  
As we know that the OpenLANE doesn't contain a SPEF extraction tool,the resulting (.spef) file can be found in the routing folder at the results folder

![Screenshot 2024-08-09 165737](https://github.com/user-attachments/assets/c7d7b690-0288-4f16-9764-b4bb7238f107)



      




     




 
  





      



 
 
 
