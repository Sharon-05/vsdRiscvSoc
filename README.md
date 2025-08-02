🛠️ Task 1: RISC-V Toolchain Setup and Verification Using WSL

🎯 Objective

Successfully install the RISC-V toolchain in WSL (Windows Subsystem for Linux), configure the environment variables, and verify that the essential binaries (gcc, objdump, and gdb) function correctly for cross-compilation development.

📋 Prerequisites

✅ WSL (Windows Subsystem for Linux) installed and configured

✅ Downloaded riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz in Windows Downloads directory

✅ Basic knowledge of Linux command line operations

🚀 Step-by-Step Implementation
Step 1: Navigate to Downloads Directory and Create Installation Path

Access the Windows Downloads directory from WSL and prepare the installation directory.
Navigate to Windows Downloads directory from WSL

Generated bash
cd /mnt/c/Users/rsdsr/Downloads


Create installation directory with proper permissions

Generated bash
sudo mkdir -p /opt/riscv
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Verify the downloaded toolchain archive exists

Generated bash
ls -la riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END
Step 2: Extract the RISC-V Toolchain

Extract the downloaded toolchain archive to the designated installation directory.

Extract the toolchain archive to /opt/riscv

Generated bash
sudo tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz -C /opt/riscv --strip-components=1
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Verify extraction was successful

Generated bash
ls -la /opt/riscv/
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Check the nested directory structure (important for PATH configuration)

Generated bash
ls -la /opt/riscv/riscv/
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END
Step 3: Configure PATH Environment Variable

Add the RISC-V toolchain binaries to the system PATH for persistent access.

Add toolchain binaries to PATH (note the nested riscv/riscv/bin structure)

Generated bash
echo 'export PATH=/opt/riscv/riscv/bin:$PATH' >> ~/.bashrc
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Reload the shell configuration to apply changes

Generated bash
source ~/.bashrc
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Verify PATH configuration

Generated bash
echo $PATH | grep riscv
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END
Step 4: Verify Toolchain Installation

Test all essential RISC-V development tools to ensure proper installation and functionality.

Check GCC compiler version

Generated bash
riscv32-unknown-elf-gcc --version
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Check objdump utility version

Generated bash
riscv32-unknown-elf-objdump --version
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Check GDB debugger version

Generated bash
riscv32-unknown-elf-gdb --version
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

Verify target architecture support

Generated bash
riscv32-unknown-elf-gcc -dumpmachine
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END

List all available RISC-V binaries

Generated bash
ls -la /opt/riscv/riscv/bin/ | grep riscv32
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Bash
IGNORE_WHEN_COPYING_END
📊 Expected Results

Upon successful completion, you should see output similar to:

GCC Version: riscv32-unknown-elf-gcc (g04696df096) 14.2.0

Target Architecture: riscv32-unknown-elf

Total Binaries: 64 RISC-V development tools available

ISA Support: rv32i2p1_m2p0_a2p1_c2p0 (RV32IMAC)

📸 Implementation Output
<img width="955" height="474" alt="Screenshot from 2025-08-01 18-10-35" src="https://github.com/user-attachments/assets/9841acc0-2949-4dc9-9497-fbe61eaba6c4" />
<img width="1272" height="788" alt="image" src="https://github.com/user-attachments/assets/ac9491f3-86f3-4603-ac90-82818fdd0246" />

<img width="1210" height="773" alt="image" src="https://github.com/user-attachments/assets/2d264aa4-229a-4320-bd84-dd05270a0ac3" />






