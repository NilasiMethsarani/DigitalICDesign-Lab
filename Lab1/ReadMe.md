RTL Synthesis Using Cadence Genus: ASIC Flow for Transceiver Design

This project demonstrates the RTL synthesis process using Cadence Genus to understand the ASIC synthesis flow. The experiment focuses on synthesizing a transceiver design written in Verilog, with key inputs including source RTL files, technology libraries, and timing constraints.
Workflow

    Project Setup:
        Created project directories and imported Verilog RTL design files into Cadence Genus.

    Elaboration and Timing Constraints:
        Ran elaboration on the design.
        Applied timing constraints such as clock frequencies, duty cycles, and pin loads.

    Synthesis:
        Performed synthesis to map the RTL design onto standard cells from the provided technology libraries.

    Design Modifications:
        Modified design parameters, including the word length for transmitted/received data, and re-compiled the design to analyze the impact on area and timing.
        Adjusted timing constraints, such as clock frequency, duty cycle, pin loads, and induced phase shifts in clock signals.

Results

    Observed changes in area and timing based on parameter adjustments.
    Analyzed timing violations and evaluated their impact on design performance.

