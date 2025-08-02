````markdown
# 🏗️🔧⚙️ RISC-V Toolchain Setup (Task 1) 🔧⚙️🏗️

---

# 🛠️ Task 1: RISC-V Toolchain Setup and Verification Using WSL

## 🎯 Objective
Install the **RISC-V toolchain** in WSL (Windows Subsystem for Linux), configure environment variables,  
and verify that essential binaries (`gcc`, `objdump`, and `gdb`) function correctly for cross-compilation.

---

## 📋 Prerequisites
- ✅ WSL installed and configured  
- ✅ Downloaded `riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz` in **Windows Downloads** directory  
- ✅ Basic knowledge of Linux command-line operations  

---

## 🚀 Step-by-Step Implementation

### 🔹 Step 1: Navigate to Downloads Directory & Create Installation Path
```bash
cd /mnt/c/Users/rsdsr/Downloads
sudo mkdir -p /opt/riscv
ls -la riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
````

### 🔹 Step 2: Extract the RISC-V Toolchain

```bash
sudo tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz -C /opt/riscv --strip-components=1
ls -la /opt/riscv/
ls -la /opt/riscv/riscv/
```

### 🔹 Step 3: Configure PATH Environment Variable

```bash
echo 'export PATH=/opt/riscv/riscv/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
echo $PATH | grep riscv
```

### 🔹 Step 4: Verify Toolchain Installation

```bash
# 🏗️ Check GCC Compiler
riscv32-unknown-elf-gcc --version

# 🔎 Check objdump Utility
riscv32-unknown-elf-objdump --version

# 🐞 Check GDB Debugger
riscv32-unknown-elf-gdb --version

# 🎯 Verify Target Architecture
riscv32-unknown-elf-gcc -dumpmachine

# 📦 List All Available RISC-V Binaries
ls -la /opt/riscv/riscv/bin/ | grep riscv32
```

---

## 📊 Expected Results

* 🖥️ **GCC Version:** `riscv32-unknown-elf-gcc (g04696df096) 14.2.0`
* 🏹 **Target Architecture:** `riscv32-unknown-elf`
* 📜 **Total Binaries:** \~64 RISC-V development tools available
* ⚙️ **ISA Support:** `rv32i2p1_m2p0_a2p1_c2p0 (RV32IMAC)`

---

✅ **Status:** Task 1 Successfully Completed 🎉

```

---

That banner (`# 🏗️🔧⚙️ RISC-V Toolchain Setup (Task 1) 🔧⚙️🏗️`) will show at the top in big bold style with emojis so the evaluator instantly knows it’s Task 1.  

Want me to also design a **footer line** (like `--- 🚀 End of Task 1 🚀 ---`) to give it a nice finishing touch?
```


📸 Implementation Output
<img width="955" height="474" alt="Screenshot from 2025-08-01 18-10-35" src="https://github.com/user-attachments/assets/9841acc0-2949-4dc9-9497-fbe61eaba6c4" />
<img width="1272" height="788" alt="image" src="https://github.com/user-attachments/assets/ac9491f3-86f3-4603-ac90-82818fdd0246" />

<img width="1210" height="773" alt="image" src="https://github.com/user-attachments/assets/2d264aa4-229a-4320-bd84-dd05270a0ac3" />






