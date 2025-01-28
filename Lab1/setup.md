# Lab 1 Synthesis Instructions

1. **Launch Terminal:**
    - In the start menu (application launcher), search and open: `Konsole`

2. **Check if you are in the right directory:**

    unix: pwd

    You should get: `/home/student/`

3. **Make a directory for your group:**

    unix: mkdir entc17_dicd/<index_no>
    unix: cd entc17_dicd/<index_no>


4. **Copy the lab files & examine the folder structure:**

    unix: cp -r ../../dicd_labs/lab1_synthesis ./
    unix: cd lab1_synthesis
    unix: ls

    Folder structure:
 
    lab1_synthesis/
    ├── input/
    ├── rtl
    ├── libs/
    ├── constraints.tcl
    ├── log/
    ├── output/
    ├── report
    └── work/
    ```
    - `input/`: Contains HDL source
    - `rtl`: Contains technology libraries (45nm GPDK)
    - `constraints.tcl`: Commands to set timing constraints
    - `log/`: Stores generated log files
    - `output/`: Output Netlist & Constraints (SDC)
    - `report`: Generated reports
    - `work/`: This is the working directory of the project

5. **Change directory to work:**
    unix: cd work
