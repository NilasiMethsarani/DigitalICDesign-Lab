# Lab 3: Place & Route

This lab focuses on the **Place & Route (P&R)** process, a critical step in the Integrated Circuit (IC) design flow before fabrication. Place & Route determines the physical layout of the IC by deciding the placement of cells and interconnecting them efficiently.

---

## **Overview of Place & Route**

Place & Route consists of two main tasks:

1. **Placement**  
   - Determines the physical location of all the elements of the design.  
   - Initial decisions are made about:
     - **Aspect Ratio**: Defines the shape of the IC core.
     - **Utilization Factors**:
       - **Core Utilization**: Ratio of core area to the total die area.
       - **Cell Utilization**: Ratio of cell area to the core area.

2. **Routing**  
   - Establishes physical interconnects between the placed elements in three main stages:
     1. **Power Routing**: Routes VDD and GND nets to distribute power across the design.
     2. **Clock Tree Synthesis (CTS)**:
        - Resynthesizes clock nets based on cell placement.
        - Balances clock skew to ensure that clock signals reach all parts of the design simultaneously.
        - Minimizes insertion delay.
     3. **Signal Routing**: Connects all other nets between the cells to complete the design.

---

## **Objectives**

- Understand the **Place & Route** process in IC design.
- Learn how to:
  - Perform cell placement based on utilization factors and aspect ratios.
  - Route power and clock signals.
  - Synthesize the clock tree and route signal nets.
- Analyze the physical layout to ensure no violations (e.g., timing, power, or design rule violations).

---

## **Experiment Workflow**

The following design flow will be followed in this laboratory experiment:

1. **Import Design Files**:  
   - Import the synthesized netlist, constraints, and technology library files.

2. **Placement**:  
   - Perform cell placement based on the core utilization and aspect ratio.

3. **Routing**:  
   - **Power Routing**: Route VDD and GND nets.  
   - **Clock Tree Synthesis (CTS)**: Synthesize the clock tree for balanced clock distribution.  
   - **Signal Routing**: Connect all other nets in the design.  

4. **Verification**:  
   - Validate the design for physical and timing violations.

---

## **Directory Structure**

The project folder for this lab is organized as follows:

