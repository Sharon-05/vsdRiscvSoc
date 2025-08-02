# RISC-V Toolchain Setup and Uniqueness Test

This repository documents the complete process of setting up the RISC-V development environment and executing a C-based uniqueness test as per the provided guidelines.

## Toolchain Setup Tasks

The setup process involves several tasks to prepare the development environment for RISC-V.

*   **Task 1: Install base developer tools:** Installed essential build prerequisites like `git`, `build-essential`, `autotools-dev`, and other required libraries.
*   **Task 2: Create a clean workspace:** Created a dedicated directory `~/riscv_toolchain` to keep all toolchain components organized.
*   **Task 3: Get a prebuilt RISC-V GCC toolchain:** Downloaded and extracted a prebuilt GCC toolchain for RISC-V to avoid a lengthy source build.
*   **Task 4: Add toolchain to your PATH:** Added the toolchain's `bin` directory to the shell's `PATH` for both current and future sessions.
*   **Task 5: Install Device Tree Compiler (DTC):** Installed the DTC, which is a common dependency in SoC/simulator flows.
*   **Task 6: Build and install Spike (RISC-V ISA simulator):** Built and installed the reference ISA simulator, Spike, to run and test compiled RISC-V programs.
*   **Task 7: Build and install the RISC-V Proxy Kernel (riscv-pk):** Built and installed the proxy kernel, which bridges compiled programs to the Spike simulator.
*   **Task 8: Ensure the cross bin directory is in PATH:** Added the nested `riscv64-unknown-elf/bin` directory to the `PATH`.
*   **Task 9 (Optional):
