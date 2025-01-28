# Setup the Project Directories

1. Launch Terminal: In the start menu (application launcher) search and open: **Konsole**

2. Check if you are in the right directory:
```sh
unix: pwd
```
   You should get: `/home/student/`

3. Navigate to your directory:
```sh
unix: cd entc20_dicd/<index_no>
```

4. Copy the lab files & examine the folder structure:
```sh
unix: cp -r ../../dicd_labs/lab3_place_and_route ./
unix: cd lab3_place_and_route
```

### Directory Structure
```
lab3_place_and_route:
|--input/       # Contains input files (.v, .sdc,.scandef, .view)
|--libs/        # Contains technology libraries (45nm GPDK)
|--log/         # Stores generated log files
|--output/      # Output files
|--report/      # Generated reports
|--pre_CTS      # Pre Clock Tree Synthesis
|--post_CTS     # Post Clock Tree Synthesis
|--post_Route   # Post Routing
|--work/        # This is the working directory of the project
```

5. Change directory to work:
```sh
unix: cd work
```
