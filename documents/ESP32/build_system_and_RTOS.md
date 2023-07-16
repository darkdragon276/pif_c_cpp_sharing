# 																																										 **Build System and RTOS for ESP32**
## Build System
### 1. Overview
- An ESP-IDF project can be seen as an amalgamation of a number of components
- **ESP-IDF** makes these `components` explicit and configurable.To do that, when a project is compiled, the build system will look up `all the components` in the ESP-IDF directories, the project directories and (optionally) in additional custom component directories.
* A simple project looks like this:

```
myProject/
  |-- build/
  |-- sdkconfig
  |-- Makefile
  |-- components/ 
  |     |-- component1/ 
  |     |     |-- component.mk
  |     |     |-- Kconfig
  |     |     |-- src1.c
  |     |-- component2/ 
  |     |     |-- component.mk
  |     |     |-- Kconfig
  |     |     |-- src1.c
  |-- main/       
        |-- src1.c
        |-- src2.c
        |-- Makefile
  
```
As you can see, a project has a `components/` subdirectory that includes `components` contained in one or more directories and `source files` for the project (default is `main`). After compilation, the project will include a `build` directory that contains all the objects, compiled libraries, and the final output binary file.
* `sdkconfig` project configuration file. This file is created/updated when `idf.py menuconfig` runs
* Component directories each contain a component `Makefile` file, and ít called `component.mk`
* Each component may also include a `Kconfig` file defining the `component configuration` options that can be set via `menuconfig`
*  Some components may also include `Kconfig.projbuild` and `project_include.cmake` files


### 2. Using MakeFile 


### 3. Configuration File
* `Kconfig`
* `menuconfig`
* `Kconfig.projbuild`
* `project_include.cmake`


## RTOS
### 1. Overview

### 2. Symmetric Multiprocessing
### 3. Scheduling
* `SMP Scheduler`
### 4. Critical Sections
