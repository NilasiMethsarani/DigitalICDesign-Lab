# Lab 2 DFT Insertion Instructions

1. **Launch Terminal:**
    - In the start menu (application launcher), search and open: `Konsole`

2. **Check if you are in the right directory:**
  
    unix: pwd
   
    You should get: `/home/student/`

3. **Navigate to your directory:**

    unix: cd entc17_dicd/<index_no>
 

4. **Copy the lab files & examine the folder structure:**

    unix: cp -r ../../dicd_labs/lab2_dft_insertion ./
    unix: cd lab2_dft_insertion
    unix: ls
 
    Folder structure:
  
    lab2_dft_insertion/
    ├── input/
    ├── rtl/
    ├── libs/
    ├── constraints.tcl
    ├── log/
    ├── output/
    ├── report/
    ├── after_scan_synthesis/
    ├── after_scan_connect/
    ├── scripts/
    └── work/
    ```
    - `input/`: Contains HDL source
    - `rtl/`: Contains technology libraries (45nm GPDK)
    - `constraints.tcl`: Commands to set timing constraints
    - `log/`: Stores generated log files
    - `output/`: Output Netlist & Constraints (SDC)
    - `report/`: Generated reports
    - `after_scan_synthesis/`: Generated reports after scan synthesis step
    - `after_scan_connect/`: Generated reports after scan connect step
    - `scripts/`: Scripts to be sourced directly
    - `work/`: This is the working directory of the project

5. **Change directory to work:**

    unix: cd work

