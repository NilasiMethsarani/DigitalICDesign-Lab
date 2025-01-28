# Using Genus for Scan Test Insertion

1. **Start Genus (command line tool):** Your terminal will change to the Genus command line.
    ```sh
    unix: genus
    ```

2. **Set the search path & include the technology libraries:**
    Since the purpose of setting the search path & including the technology libraries was discussed in the previous laboratory experiment, we shall execute these commands at once by sourcing the given `../scripts/setup.tcl` file.
    ```sh
    genus: source ../scripts/setup.tcl
    ```
    You may observe the contents of `setup.tcl` using `vim`. It contains the following commands:
    ```sh
    genus: set_db init_lib_search_path [list ../input/libs/gsclib045/lef ../input/libs/gsclib045/timing ../input/libs/gsclib045/qrc/qx]
    genus: set_db library {slow_vdd1v0_basicCells.lib fast_vdd1v0_basicCells.lib}
    genus: set_db lef_library {gsclib045_tech.lef gsclib045_macro.lef gsclib045_multibitsDFF.lef}
    genus: set_db qrc_tech_file gpdk045.tch
    ```

3. **Read the RTL design files into Genus:**
    ```sh
    genus: read_hdl [glob ../input/rtl/*.v]
    ```

4. **Elaborate the design:**
    ```sh
    genus: elaborate uart_top
    ```

5. **Uniquify the top module:**
    ```sh
    genus: uniquify uart_top
    ```

6. **Set timing constraints in Genus:**
    ```sh
    genus: source ../input/constraints.tcl
    ```

    Now that we have set the target libraries, read the design files, and set the design constraints, we may now proceed to the DFT insertion steps.

7. **Set the DFT scan style to configure the DFT rule checker:**
    This is due to the fact that DFT rules vary with different scan styles. Genus supports two scan styles – Muxed Scan Style and Clocked Level-Sensitive-Scan (LSS) Style. We will be using the Muxed Scan Style in this experiment.
    ```sh
    genus: set_db dft_scan_style muxed_scan
    ```
    Note: We use the `set_db` command to set attributes in the common UI mode in Genus.

8. **Set the prefix for names of additional modules/ports:**
    These will be automatically created during the DFT insertion process to accommodate test logic. We will set this prefix name as `dft_`.
    ```sh
    genus: set_db dft_prefix dft_
    ```

9. **Define the shift_enable signal:**
    Since the muxed-scan style uses shift_enable signals to switch scan flip-flops from functional mode to scan-shift mode, it is recommended that we define these signals before running the DFT rule checker. The following command defines an active-high shift_enable pin with the name `SE` and creates a port for it.
    ```sh
    genus: define_shift_enable -name SE -active high -create_port SE
    ```

10. **Run the DFT rule checker:**
    ```sh
    genus: check_dft_rules
    ```
    The DFT rule checker checks all flip-flops to determine if clock pins to the flip-flops can be controlled and if asynchronous set/reset pins (if available) to the flip-flops can be held to their non-controlling value during scan-shift mode. It is essential that there are no DFT violations at this step as any flip-flops with DFT violations will not be affected by DFT insertion commands which will be executed in the subsequent steps.
    If all steps have been followed correctly up until this point, we should not see any DFT violations.

11. **Synthesize the design into the generic logic netlist and then map to the technology library:**
    In this step, all the non-scannable flip-flops will be replaced by scannable flip-flops.
    Synthesize to generic logic with medium effort
    ```sh
    genus: set_db syn_generic_effort medium
    genus: syn_generic
    ```
    Map to technology library and re-synthesize with medium effort
    ```sh
    genus: set_db syn_map_effort medium
    genus: syn_map
    ```

12. **Write the scan synthesized netlist as `uart_top_1.v`:**
    ```sh
    genus: write_hdl > ../output/uart_top_1.v
    ```

13. **Generate the reports after scan synthesis:**
    These reports will be necessary for comparisons later.
    ```sh
    genus: report_area > ../report/after_scan_synthesis/area.log
    genus: report_timing -nworst 10 > ../report/after_scan_synthesis/timing.log
    genus: report_port * > ../report/after_scan_synthesis/ports.log
    genus: report_power > ../report/after_scan_synthesis/power.log
    ```

14. **Set scan configuration:**
    In this step, we will define the scan chains according to our requirements. Note that the design contains two transceivers which operate on two clock domains. Therefore, we need at least one scan chain per clock domain to cover the entire design. Execute the following commands to define two scan chains – one per clock domain.
    ```sh
    genus: define_scan_chain -name top_chain_a -sdi scan_in_a -sdo scan_out_a -non_shared_output -create_ports -domain clk_a
    genus: define_scan_chain -name top_chain_b -sdi scan_in_b -sdo scan_out_b -non_shared_output -create_ports -domain clk_b
    ```

15. **Preview the scan chains:**
    Here, we will be able to see a summary of the scan chains we defined in the previous step. It will show the name, scan input port, scan output port, clock domain, sensitive edge, and the number of scannable flip-flops in each chain.
    ```sh
    genus: connect_scan_chains -preview -auto_create_chains
    ```

16. **Connect scan chains:**
    This command will connect the scan inputs (SI) of the scannable flip-flops to the respective output ports of other scannable flip-flops or the scan input port (if it is the first scannable flip-flop in the chain).
    ```sh
    genus: connect_scan_chains -auto_create_chains
    ```
    Note: This process is also referred to as “Scan Stitching”.

17. **Perform incremental synthesis to generate the netlist of the scan connected design:**
    ```sh
    genus: syn_opt -incr
    ```

18. **Perform DFT rule check after scan connecting:**
    ```sh
    genus: check_dft_rules
    ```

19. **Report scan setup and scan chain information:**
    You may open and read their contents using `vim`.
    ```sh
    genus: report_scan_setup > ../report/scan_setup.log
    genus: report_scan_chains > ../report/scan_chains.log
    ```

20. **Write the DFT (scan test) inserted netlist and constraints:**
    ```sh
    genus: write_hdl > ../output/uart_top_2.v
    genus: write_sdc > ../output/uart_top_2.sdc
    ```

21. **Write the scanDEF file:**
    The scanDEF file describes the scan chain configuration (architecture) as the set of re-orderable scan chains present in the scan-mapped netlist. This file is necessary for the Place&Route tool to re-order scan chains.
    ```sh
    genus: write_scandef > ../output/uart_top_2_scanDEF.scandef
    ```

22. **Generate the reports after scan connect:**
    ```sh
    genus: report_area > ../report/after_scan_connect/area.log
    genus: report_timing -nworst 10 > ../report/after_scan_connect/timing.log
    genus: report_port * > ../report/after_scan_connect/ports.log
    genus: report_power > ../report/after_scan_connect/power.log
    ```

23. **Write the scripts required for the ATPG tool:**
    This command generates a script and the data files required to run an ATPG analysis using the Cadence Modus Test software (not covered in this experiment).
    ```sh
    genus: write_dft_atpg -library ../input/libs/gsclib


    # ATPG Tool Scripts and Data Files
24.**Observe the synthesized design in GUI:**
    Try to select the designs in hierarchy (left), right-click, schematic view, in New. You can also report power, timing, etc.
    ```sh
    genus: gui_show
    ```
