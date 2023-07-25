# **Build System and RTOS for ESP32**

## Build System

### 1. Overview
This is the overview of system build

![10](/documents/ESP32/assets/build_system_overview.png "This is the overview of system build" )

- An ESP-IDF project can be seen as an amalgamation of a number of components
- **ESP-IDF** makes these `components` explicit and configurable.To do that, when a project is compiled, the build system will look up `all the components` in the ESP-IDF directories, the project directories and (optionally) in additional custom component directories.

* A simple project looks like this:

```C
myProject/
├── CMakeLists.txt   //*
├── sdkconfig       //Created/updated when `idf.py menuconfig` run
├── components/
│   ├── component1/
│   │   ├── CMakeLists.txt
│   │   ├── Kconfig           //File defining the component configuration options
│   │   ├── src1.c
│   │   └── include/
│   │       └── component1.h
│   ├── component2/
│   │   ├── CMakeLists.txt
│   │   ├── Kconfig
│   │   ├── src2.c
│   │   └── include/
│   │       └── component2.h
├── main/           //*
│   ├── CMakeLists.txt
│   ├── src1.c
│   └── src2.c
└── build/     //*Contains all the objects, compiled libraries, and the final output binary file.

```
**INSTALL ESP-IDF**
- `Windown`

Step1: Download one of ESP-IDF Tools Installers\
 [Windows Installer Download](https://dl.espressif.com/dl/esp-idf/?idf=4.4).

Step2: Launching ESP-IDF Environment.
At the end of the installation process you can check out option `Run ESP-IDF PowerShell Environment` or `Run ESP-IDF Command Prompt (cmd.exe)`

`Run ESP-IDF PowerShell Environment`:
<img src="./assets/esp-idf-installer-screenshot-powershell.png" alt="Completing the ESP-IDF Tools Setup Wizard with Run ESP-IDF PowerShell Environment" title="Completing the ESP-IDF Tools Setup Wizard with Run ESP-IDF PowerShell Environment" style="width: 70%; height: auto;">
<br>
<img src="./assets/esp-idf-installer-powershell.png" alt="ESP-IDF PowerShell" title="ESP-IDF PowerShell" style="width: 70%; height: auto;">

`Run ESP-IDF Command Prompt (cmd.exe)`:
<img src="./assets/esp-idf-installer-screenshot.png" alt="Completing the ESP-IDF Tools Setup Wizard with Run ESP-IDF Command Prompt (cmd.exe)" title="Completing the ESP-IDF Tools Setup Wizard with Run ESP-IDF Command Prompt (cmd.exe)" style="width: 70%; height: auto;">
<br>
<img src="./assets/esp-idf-installer-command-prompt.png" alt="ESP-IDF Command Prompt" title="ESP-IDF Command Prompt" style="width: 70%; height: auto;">

* `Ubuntu`

**Step1**: Install Prerequisites
Run command below
```
sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
```
**Step2**: Get ESP-IDF\
To build applications for the ESP32, you need the software libraries provided by Espressif in [ESP-IDF repository](https://github.com/espressif/esp-idf ).\
Open Terminal, and run the following commands:
```
mkdir -p ~/esp
cd ~/esp
git clone --recursive https://github.com/espressif/esp-idf.git
```
**Step 3**. Set up the Tools
```
cd ~/esp/esp-idf
./install.sh esp32
```
**Step 4**. Set up the Environment Variables
Open the shell configuration file `.bashrc `
```
nano ~/.bashrc
```
Scroll down to the end of the file and copy and paste the alias command below:
```
alias get_idf='. $HOME/esp/esp-idf/export.sh'

```
Save and exit the text editor by pressing **Ctrl + X**, then press **Y** to confirm saving changes, and **Enter** to set the file name.\
After that, run this command to apply change
```
source ~/.bashrc
```
**Step5**: Almost done\
Now you can run the `get_idf` command in any terminal to set up or refresh the ESP-IDF environment.



### 2. Using MakeFile

#### Project CMakeLists File

A top-level project CMakeLists.txt file

- Minimal project:
  > ```C
  > cmake_minimum_required(VERSION 3.16)
  > include($ENV{IDF_PATH}/tools/cmake/project.cmake)
  > project(myProject)
  > ```

Optional Project Variables:

- `COMPONENT_DIRS`: Directories to search for components.
- `EXTRA_COMPONENT_DIRS`: An optional list of additional directories to search for components.
- `COMPONENTS`: A list of component names to build into the project.

To set these variables, use the cmake set command ie `set(VARIABLE "VALUE")`.
The `set()` commands should be placed after the `cmake_minimum(...)` line but before the `include(...)` line.

Example:

```C
cmake_minimum_required(VERSION 3.16)
include($ENV{IDF_PATH}/tools/cmake/project.cmake)

set(EXTRA_COMPONENT_DIRS ${CMAKE_CURRENT_LIST_DIR}/component1 ${CMAKE_CURRENT_LIST_DIR}/component2)

project(myProject)

```

#### Component CMakeLists Files
- Minimal Component CMakeLists:
```C
idf_component_register(SRCS source_files...
                       INCLUDE_DIRS include_directories...
                       REQUIRES required_components...
                       PRIV_REQUIRES private_required_components...)
```
* `SRCS` is a list of source files (`*.c`, `*.cpp`, `*.cc`, `*.S`). These source files will be compiled into the component library.

* `INCLUDE_DIRS` is a list of directories to add to the global include search path for any component which requires this component, and also the main source files.
* `REQUIRES` gives the list of components required by the public interface of this component.
* `PRIV_REQUIRES` gives the list of components required by the private interface of this component.

Example:
```c
//component 1
idf_component_register(SRCS "src1.c"
                       INCLUDE_DIRS "include")
```
**Note**
In CMake terms, `REQUIRES` & `PRIV_REQUIRES` are approximate wrappers around the CMake functions `target_link_libraries(... PUBLIC ...)` and `target_link_libraries(... PRIVATE ...)`.
### 3. Configuration File

#### Kconfig
Including configuration options for components
Example:
```Python
menu "This is a memnuconfig for component"
    # some commands to control
endmenu
```
Result:
<img src="./assets/config_component.png" alt="config_component.png" style="zoom:80%;" />

#### Kconfig.projbuild
 Including configuration options at the top level of menuconfig.\
Example:
```Python
menu "This is a memnuconfig for main"
    # some commands to control
endmenu
```
Result:
<img src="./assets/Config_main.png" alt="Config_main.png" style="zoom:80%;" />

## RTOS

### 1. Overview

- The original FreeRTOS( Vanilla FreeRTOS) only supports **single core**. In order to use it for **dual core**, we need to use `ESP-IDF FreeRTOS`, which is based on Vanilla FreeRTOS v10.4.3.

### 2. Symmetric Multiprocessing(SMP)

ESP-IDF provides a dual core SMP (Symmetric Multiprocessing).\
<img src="./assets/Symmetric_Multiprocessing_SMP_Architecture.png" alt="Symmetric_Multiprocessing_SMP_Architecture" style="zoom:60%;" />

- SMP (Symmetric Multiprocessing) is a computing architecture where two or more identical CPUs (cores) are connected to a single shared main memory and controlled by a single operating system.

  - Has multiple cores running independently. Each core has its own register file, interrupts, and interrupt handling.

  - Presents an identical view of memory to each core. Thus a piece of code that accesses a particular memory address will have the same effect regardless of which core it runs on.

#### SMP on an ESP Target

ESP targets (such as the ESP32, ESP32-S3) are dual core SMP SoCs.

- Two identical cores known as CPU0 (i.e., Protocol CPU or **PRO_CPU**) and CPU1 (i.e., Application CPU or **APP_CPU**)
- Cross-core interrupts that allow one CPU to trigger and interrupt on another CPU. This allows cores to **signal each other**.

<img src="./assets/esp32_block_diaram.png" alt="esp32_block_diaram" style="zoom:30%;" />

**NOTE:**

- A thread that is pinned to a particular core will only be able to run on that core
- A thread that is unpinned will be allowed to switch between cores during execution instead of being pinned to a particular core.

### 3. Task

#### **Creation**

In an SMP system, tasks need to be assigned a particular affinity.

- `xTaskCreatePinnedToCore()` creates a task with a particular core affinity. The task’s memory is dynamically allocated.
- `xTaskCreateStaticPinnedToCore()` creates a task with a particular core affinity. The task’s memory is statically allocated (i.e., provided by the user).

The valid values for core affinity are: `PinnedToCore` `xCoreID`

- `0` which pins the created task to CPU0

- `1` which pins the created task to CPU1

- `tskNO_AFFINITY` which allows the task to be run on both CPUs

**NOTE**

- ESP-IDF FreeRTOS still supports the Vanilla FreeRTOS. That means **always tskNO_AFFINITY** every time create( `xTaskCreate()`,`xTaskCreateStatic()`).
- In ESP-IDF FreeRTOS, the task stack sizes are specified in **bytes**. `ulStackDepth`

#### **Deletion**

ESP-IDF FreeRTOS provides the same vTaskDelete() function. However, due to the dual core nature, there are some behavioral differences when calling vTaskDelete() in ESP-IDF FreeRTOS:

- When deleting a task that is pinned to the other core, that task’s memory is always freed by the idle task of the other core (due to the need to clear FPU registers).

- When deleting a task that is currently running on the other core, a yield is triggered on the other core and the task’s memory is freed by one of the idle tasks (depending on the task’s core affinity)

- A deleted task’s memory is freed immediately if…

- The tasks is currently running on this core and is also pinned to this core

- The task is not currently running and is not pinned to any core

The best ways to use `vTaskDelete()` that is only ever called on tasks in a **known state**. For example:

- Tasks self deleting (via `vTaskDelete(NULL)`).
- Tasks placing themselves in the suspend state (via `vTaskSuspend()`) before being deleted by another task.

### 4. Scheduling

- **SMP Scheduler**

  There are several differences in scheduling behavior between `vanilla` and `ESP-IDF FreeRTOS` such as differences in Round Robin scheduling, scheduler suspension, and tick interrupt synchronicity.vTaskSwitchContext()vTaskSwitchContext().

Vanilla FreeRTOS:
<img src="./assets/freertos_ready_task_list.png" alt="freertos_ready_task_list.png" style="zoom:80%;" />

ESP-IDF FreeRTOS:

<img src="./assets/IDFfreertos_ready_task_list-smp.png" alt="freertos_ready_task_list.png" style="zoom:80%;" />

- Therefore when PRO_CPU calls the scheduler, it will only consider the tasks in blue or purple.
- Whereas when APP_CPU calls the scheduler, it will only consider the tasks in orange or purple.

#### **Round Robin Scheduling**

- Although each TCB has an in ESP-IDF FreeRTOS, the linked list of each priority only has a single `xCoreID` `pxIndex`.

- Therefore when the scheduler is called from a particular core and traverses the linked list, it will skip all TCBs pinned to the other core and point the `pxIndex` at the selected task. If the other core then calls the scheduler, it will traverse the linked list starting at the TCB immediately after pxIndex

- Therefore, TCBs skipped on the previous scheduler call from the other core would not be considered on the current scheduler call

This is an example:

> ```
> Processor A: TCB-A1 (priority 1) -> TCB-A2 (priority 2) -> TCB-A3 (priority 3)
> Processor B: TCB-B1 (priority 1) -> TCB-B2 (priority 2) -> TCB-B3 (priority 3)
> ```

Result:

- When Processor A's scheduler is called, it will select the highest priority ready task for its core, which is TCB-A1 (priority 1). It will then execute TCB-A1.
- When Processor B's scheduler is called, it will also select the highest priority ready task for its core, which is TCB-B1 (priority 1). However, since TCB-B1 has already been executed by Processor A, Processor B's scheduler will skip TCB-B1 and select the next highest priority ready task, which is TCB-B2 (priority 2). It will then execute TCB-B2.

#### **Scheduler Suspension**

In ESP-IDF FreeRTOS, `xTaskSuspendAll()` will only prevent calls of `vTaskSwitchContext()` from switching contexts on **the core that called for the suspension**. Hence if PRO_CPU calls vTaskSuspendAll(), APP_CPU will still be able to switch contexts.

**Note**

- In general, it’s better to use other RTOS primitives like mutex semaphores to protect against data shared between tasks, rather than vTaskSuspendAll().

#### **Tick Interrupt Synchronicity**

- In ESP-IDF FreeRTOS, **PRO_CPU** is responsible for incrementing the shared tick count.
- However tick interrupts to each core might not be synchronized (**same frequency but out of phase**) hence when PRO_CPU receives a tick interrupt, APP_CPU might not have received it yet.

**Note**

- Task delays should **NOT** be used as a method of synchronization between tasks in ESP-IDF FreeRTOS. Instead, consider using a counting semaphore to unblock multiple tasks at the same time.

### 5. Critical Sections

- Vanilla FreeRTOS implements critical sections by using `portDISABLE_INTERRUPTS()` which disables the scheduler
- ESP32 has no hardware method for cores to disable each other’s interrupts. Calling `portDISABLE_INTERRUPTS()` will have no effect on the interrupts of the other core

For this reason, ESP-IDF FreeRTOS implements critical sections using special mutexes, referred by `portMUX_Type` objects

|                   | Vanilla FreeRTOS                                                                                                     | ESP-IDF FreeRTOS                                                                                                                               |
| ----------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Implements        | by disabling interrupts                                                                                              | using special mutexes                                                                                                                          |
| Section functions | `taskENTER_CRITICAL()`<br>`taskEXIT_CRITICAL()`<br>`taskENTER_CRITICAL_FROM_ISR()`<br>`taskEXIT_CRITICAL_FROM_ISR()` | `taskENTER_CRITICAL(&spinlock)`<br>`taskEXIT_CRITICAL(&spinlock)`<br>`taskENTER_CRITICAL_ISR(&spinlock)`<br>`taskEXIT_CRITICAL_ISR(&spinlock)` |

- Allocating a static spinlock and initializing it using portMUX_INITIALIZER_UNLOCKED

```C
// Statically allocate and initialize the spinlock
static portMUX_TYPE my_spinlock = portMUX_INITIALIZER_UNLOCKED;

void some_function(void)
{
    taskENTER_CRITICAL(&my_spinlock);
    // We are now in a critical section
    taskEXIT_CRITICAL(&my_spinlock);
}
```

- Allocating a dynamic spinlock and initializing it using portMUX_INITIALIZE()

```C
// Allocate the spinlock dynamically
portMUX_TYPE *my_spinlock = malloc(sizeof(portMUX_TYPE));
// Initialize the spinlock dynamically
portMUX_INITIALIZE(my_spinlock);



taskENTER_CRITICAL(my_spinlock);
// Access the resource
taskEXIT_CRITICAL(my_spinlock);
```
