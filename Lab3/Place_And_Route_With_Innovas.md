Here's a concise guide based on your steps for **Using Innovus for Place and Route**:

---

### **Innovus Place and Route Workflow**

#### **1. Start Innovus**
- Launch **`innovus.xlaunch`**. The Innovus GUI and terminal will appear.

#### **2. Import the Design**
- Navigate to **File > Import Design**.
  - **Netlist**:
    - Select *Verilog*.
    - Browse to `../input/uart_top_2.v`.
    - Set **Top Cell** to auto-assign.
  - **Technology Libraries**:
    - Add LEF files from `../input/libs/gsclib045/lef/` in order:
      1. `_tech.lef`
      2. `_multibitsDFF.lef`
      3. `_macro.lef`
  - **Power**:
    - Power Net: `VDD`
    - Ground Net: `GND`
  - **Analysis Config**:
    - Browse to `../input/uart_top.view`.
  - Save and click **OK**.

#### **3. Read ScanDEF File**
```bash
Innovus: defIn ../input/uart_top_2_scanDEF.scandef
```

#### **4. Set Process Technology**
```bash
Innovus: setDesignMode -process 45
```

#### **5. Specify Floorplan**
- Go to **Floorplan > Specify Floorplan**.
  - Aspect Ratio: **1.0**
  - Core Utilization: **0.4**
  - Core Margins: **5 μm**.
  - Click **OK**.

#### **6. Add Power Rings and Stripes**
- **Power Rings**:
  - Go to **Power Planning > Add Rings**.
  - Nets: `VDD GND`.
  - Config:
    - **Width**: `1.0`
    - **Spacing**: `0.8`
    - **Offset**: `0.8`
- **Horizontal Power Stripes** (Metal 7):
  - Go to **Power Planning > Add Stripes**.
  - Nets: `VDD GND`.
  - **Width**: `1.0`, **Spacing**: `0.8`, **Start/End**: `15`.
- **Vertical Power Stripes** (Metal 8): Repeat with **Direction: Vertical**.

#### **7. Pin Placement**
- Go to **Edit > Pin Editor**.
  - Assign pins to edges with spacing **5 μm**.
  - Update **clk_a** and **clk_b** to `CLOCK` type.

#### **8. Save Design**
- Save as **uart_top_prePlacement**.

#### **9. Standard Cell Placement**
- Go to **Place > Place Standard Cell**.
  - Run **Full Placement**.
  - Exclude **IO Pins** placement.

#### **10. Gate Count Report**
- Go to **File > Report > Gate Count**.
- Save as **uart_top_optPlace.gateCount**.

#### **11. Pre-CTS Setup Timing Analysis**
- Go to **Timing > Report Timing**.
  - Select:
    - **Stage**: Pre-CTS.
    - **Type**: Setup.
    - Output Directory: `../report/pre_CTS`.

#### **12. Route Power Nets**
- Go to **Route > Special Route**.
  - Nets: `VDD GND`.

#### **13. Clock Tree Synthesis (CTS)**
- Run the following command:
```bash
Innovus: ccopt_design
```
- Take screenshots:
  - Design without nets.
  - Clock nets.

#### **14. Scan Chain Reordering**
- Go to **Place > Scan Chain > Reorder**.
  - Enable **Clock Tree Aware**.

#### **15. Post-CTS Timing Analysis**
- Go to **Timing > Report Timing**.
  - Select:
    - **Stage**: Post-CTS.
    - **Type**: Hold.
    - Output Directory: `../report/post_CTS`.

#### **16. Signal Routing**
- Navigate to `Route > NanoRoute > Route`.
- Leave all parameters at their default settings.
- Click **OK** to route the signal nets.

#### **17. Post-Route Timing Analysis**
- Ensure that timing constraints are met post-routing:
  - Set the analysis type to **On-Chip Variation (OCV)** using the command:
    ```bash
    Innovus: setAnalysisMode -analysisType onChipVariation
    ```
  - Generate timing reports:
    - Navigate to `Timing > Report Timing`.
    - Verify that there are **no timing violations**.

#### **18. Filler Cell Placement**
- Filler cells are required to maintain n-well continuity and prevent sagging during fabrication:
  - Go to `Place > Physical Cell > Add Filler`.
  - In the **Cell Name(s)** field, select all cells with the “FILL” prefix.
  - Set **Prefix** to `FILLER`.
  - Enable **Do DRC**.
  - Enable **Fit Gap** to use combinations of filler cells for empty gaps.
  - Leave other parameters at default and click **OK**.
- Your design should resemble the post-filler design shown in Figure 8.

#### **19. Verification**
- Ensure there are no **geometry violations** (e.g., cell overlaps, spacing, minimum area) or **connectivity issues** (e.g., open or unrouted nets):
  - **Geometry Verification**:
    - Go to `Verify > Verify Geometry`.
    - In the advanced tab, set the location to `../report/uart_top.geom.rpt`.
    - Click **OK** to generate the report.
  - **Connectivity Verification**:
    - Go to `Verify > Verify Connectivity`.
    - Set the location to `../report/uart_top.conn.rpt`.
    - Click **OK** to generate the report.
- Ensure **no violations** are present in the reports.

#### **20. GDSII Export**
- Export the design to the **GDSII format** for fabrication:
  - Go to `File > Save > GDS/OASIS`.
  - Configure the export settings:
    - **Output Format**: GDSII/Stream.
    - **Output File**: `../output/uart_top.gds`.
    - Select the **map file** from the input folder (`streamOut.map`).
    - Change the **Library Name** to `uart_lib`.
    - Leave other parameters at default.
  - Click **OK** to generate the GDSII file.

---

### **Final Notes**
- Ensure that all verification checks pass before exporting the design.
- The `uart_top.gds` file is the final output required for fabrication.
