## Using Genus

1. **Start Genus (command line tool):** Your terminal will change to the Genus command line.

    unix: genus


2. **Set the search path & include the technology libraries:**
    These libraries are provided by the foundry. However, for this practical, we utilize educational 45nm Generic Libraries provided by Cadence. This library is provided only for teaching digital IC design. It is strictly prohibited to use them for any kind of commercial purposes.
    - Library Timing (.lib) files specify timing (cell delay, cell transition time, setup and hold time requirement) and power characteristics of standard cells. Slow and fast libraries characterize standard cells with maximum and minimum signal delays, which could occur from process variations.
    - Tch files are binary files that accurately characterize library elements, including capacitance and resistance.
    - Library Exchange Format (LEF) specifies design rules, metal capacitances, layer information, etc.

    genus: set_db init_lib_search_path [list ../input/libs/gsclib045/lef ../input/libs/gsclib045/timing ../input/libs/gsclib045/qrc/qx]
    genus: set_db library {slow_vdd1v0_basicCells.lib fast_vdd1v0_basicCells.lib}
    genus: set_db lef_library {gsclib045_tech.lef gsclib045_macro.lef gsclib045_multibitsDFF.lef}
    genus: set_db qrc_tech_file gpdk045.tch


3. **Read the RTL design files into Genus:**

    genus: read_hdl [glob ../input/rtl/*.v]


4. **Elaborate the design:**
    During elaboration, the tool reads through your design, reports syntax errors, creates design hierarchy, applies parameters, and connects signals. You need to provide the name of the top module (uart_top here).

    genus: elaborate uart_top


5. **Check design and output the log to a file:**
    This reports any bugs: undriven pins, unloaded ports, unresolved references, empty modules, etc.

    genus: check_design > ../log/check_design.log


6. **Uniquify the top module:**
    This eliminates sharing of subdesigns between instances.

    genus: uniquify uart_top


7. **Set timing constraints in Genus:**
    Open the `constraints.tcl` file with `vim` (in a separate terminal) and read it (then quit vim with `[esc]`, `:q!`, `[enter]`). You can observe the period, duty cycle of two clocks, input delay of each input port, etc.

    unix: vim ../input/constraints.tcl
    genus: source ../input/constraints.tcl


8. **Synthesize the design:**
    `to_mapped` specifies the design to stop after optimizing RTL and mapping to the given technology. We are using medium effort. With high effort, you can get better results, but it runs for longer time.

    genus: synthesize -to_mapped -effort m


9. **Write netlist & constraints:**
    Netlist is another Verilog file (open and read it!) where your behavioral source code has been mapped into the standard cells provided by the foundry. SDC file contains constraints needed by the place & route tools.

    genus: write -mapped > ../output/uart_top.v
    genus: write_sdc > ../output/uart_top.sdc
    unix: vim ../output/uart_top.v
    unix: vim ../output/uart_top.sdc


10. **Generate and read reports:**
    You can see the area estimate (mm²), power estimate, any timing violations, etc.

    genus: report_area > ../report/area.log
    genus: report_timing -nworst 10 > ../report/timing.log
    genus: report_port * > ../report/ports_final.log
    genus: report_power > ../report/power.log
    unix: vim ../report/area.log
    unix: vim ../report/timing.log
    unix: vim ../report/ports_final.log
    unix: vim ../report/power.log


11. **Observe the synthesized design in GUI:**
    Try to select the designs in hierarchy (left), right-click, schematic view, in New. You can also report power, timing, etc.

    genus: gui_show
