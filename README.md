# VSD-SOC_DESIGN_AND_Planning
This repository is an exercise to explore OPENLANE - open-source tool for backend of VLSI design. It uses an example design present in the OPENLANE tool - Picorv32a to go over the backend flow. Create a custom standard cell - Inverter. Characterize the inverter and merge it into the example design picorv32a and run the entire backend flow.
We will go over-
1. [A Crash course on VLSI Design Flow:.] (## A Crash course on VLSI Design Flow:)
2. Exploring OPENLANE- Getting familiar with the tool
3. Characterizing a standard cell
4. Merging the standard cell into the example design
5. Running the Backend Flow on the example design merged with the standard cell
   - Synthesis
   - Timing Analysis on Synthesis Netlist
   - floorplan
   - Placement
   - Timing Analysis on Placement Netlist
   - Clock Tree Synthesis
   - Timing Analysis on Clock Tree Synthesis Netlist
   - PDN
   - Routing
   - Timing Analysis after Routing
   - DRC
      
  
## A Crash course on VLSI Design Flow:
### Frontend of Design
Designing a custom System on Chip (SoC) involves a detailed and multi-step process that is typically divided into frontend and backend stages within the semiconductor industry. The frontend of SoC/VLSI design encompasses several critical tasks, starting with the creation of a Design Specification that outlines the requirements and goals of the chip. This is followed by Design Architecture, where the overall structure and components of the chip are defined. RTL Design involves coding the chip's functionality in a Hardware Description Language (HDL), such as Verilog or VHDL.

Verification is a crucial step in ensuring that the design functions correctly and meets the specified requirements. Synthesis then translates the RTL description into a gate-level netlist, which represents the logical connections between the various components of the chip. Post-synthesis verification ensures that the netlist accurately reflects the intended design and functionality.

### Backend of Design
Transitioning to the backend phase, the focus shifts to preparing the design for fabrication by foundries to create the physical SoC. Before delving into the backend flow, it's essential to have a thorough understanding of the chosen foundry's processes and requirements. Each foundry has specific rules and guidelines that must be followed during the layout and design phase to ensure compatibility with their manufacturing capabilities.

Semiconductor fabrication involves lithography, where masks are used as stencils to create intricate patterns on the semiconductor material. These masks must adhere to the foundry's rules to ensure successful fabrication. The backend flow in this repository utilizes Process Design Kits (PDKs), the Skywater PDK  to ensure that the design meets the foundry's specifications.

The backend flow begins with floorplanning, where the area, position, and size of the core design are determined. This is followed by placement, where standard cells are placed based on the constraints established during floorplanning. Clock Tree Synthesis (CTS) is then performed to create an efficient clock distribution network, essential for synchronous operation. Power distribution network setup ensures that the chip receives power uniformly and efficiently. Signal routing connects the various components of the chip, ensuring proper data flow and communication. Finally, the signoff stage involves rigorous testing and verification to ensure that the design meets all requirements and specifications before proceeding to fabrication.

This repository explores the use of open-source tools to navigate the backend flow of VLSI Design, incorporating gate-level netlists, standard cell libraries designed using PDKs, and the Skywater PDK for fabrication considerations. It delves into detailed steps such as floorplanning, placement, CTS, power distribution network setup, signal routing, and signoff, providing a comprehensive overview of the backend design process for custom SoCs.

[image credits - https://www.linkedin.com/posts/shilpi-garg2328_full-vlsi-design-flow-easily-understandable-activity-7052706109693861888-QqRW]


![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/44cc0e3d-3741-425b-9531-cd9f3ae2de83)

 ## Exploring OPENLANE- Getting familiar with the tool
 
Openlane is an aggregation of tools used to perform Placement and Route. As shown in the image below:

[image credits - https://openlane.readthedocs.io/en/latest/flow_overview.html]
![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/c6595b73-b3c6-43d6-a519-5d6827bb7f5d)

Installation guide for OPENLANE can be found here https://openlane.readthedocs.io/en/latest/getting_started/installation/installation_ubuntu.html

Openlane runs in a container, I have used docker to run Openlane.
To start openlane :-
- Got to Openlane directory
- invoke the docker > `docker`
- `./flow.tcl -interactive`   invoke flow.tcl and run it in interactive mode. Openlane 1 uses flow.tcl as a guide to execute the Backend flow.
-`package require openlane 0.9`
- `prep -design picorv32a`    we are using the RISCV processor example present in the design directory. This statement tells the tool we are working on picorv32a
  
  ![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/308923d7-287c-4061-a7eb-2632d335790d)

`run_synthesis`
`run_floorplan`

Finding the ratio of D Flip Flops to the number of cells - 1656/14876 = 0.11

![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/bd14eb15-ed6b-440b-93b7-619ca2092c28)

Die Area

![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/ac8c0b9d-685a-4e10-b75d-814004e0700a)

To see the die in the magic GUI, we need to open the tool outside the docker

- `magic -T [techfile] lef read [lef file] def read [def_file]
- Tech file will be found in the pdk/sky130A/libs.tech/magic/sky130A.tech
- Lef file will be found in picorv32a/runs/[LATEST_RUN]/tmp/merged.lef
- Def file in picorv32a/runs/[LATEST_RUN]/results/placement/picorv32a.placement.def
- 
![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/9405eb1d-f591-4e65-b375-b257f91caf01)

A few Magic hot keys
V to center the layout
Z to zoom in
X to expand
S to select objects

![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/c1c3132f-b0d8-4f62-9fd9-512162346f75)

![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/29780d9a-1a84-43ca-95a7-ac4b1686a99a)

We have verified the floorplan was successful

Moving on to placement
`run_placement`

![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/9f63fb37-2ab7-47b8-9590-f147ff8a95bb)

![image](https://github.com/Komal-design-vlsi/VSD-SOC_DESIGN_AND_Planning/assets/35945573/e003b610-661b-4f20-a2e5-47daa9ecd1f7)

## 3. Characterizing a standard cell


