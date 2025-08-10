````markdown
🏗️🔧⚙️ RISC-V Toolchain Setup & Uniqueness Test ⚙️🔧🏗️

🎯 Objective
Install the RISC-V toolchain in WSL (Windows Subsystem for Linux), configure environment variables,
and verify that essential binaries (gcc, objdump, and gdb) function correctly for cross-compilation.

🛠️🏗️Task 1 — Install Base Developer Tools
Install common prerequisites for RISC‑V toolchain and Spike.  
```bash
sudo apt-get update
sudo apt-get install -y git vim autoconf automake autotools-dev curl \
  libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex \
  texinfo gperf libtool patchutils bc zlib1g-dev libexpat1-dev gtkwave
````

---

🗂️ Task 2 — Create Workspace & Capture Home Path
Keep everything in `~/riscv_toolchain` for consistency.

```bash
cd
pwd=$PWD
mkdir -p riscv_toolchain
cd riscv_toolchain
```

---

📥 Task 3 — Get Prebuilt RISC‑V GCC Toolchain

```bash
wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"
tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz
```

---

🔗 Task 4 — Add Toolchain to PATH

```bash

echo 'export PATH=$HOME/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

📦 Task 5 — Install Device Tree Compiler (DTC)

```bash
sudo apt-get install -y device-tree-compiler
```

---

🖥️ Task 6 — Build and Install Spike (ISA Simulator)

```bash
cd $pwd/riscv_toolchain
git clone https://github.com/riscv/riscv-isa-sim.git
cd riscv-isa-sim
mkdir -p build && cd build
../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14
make -j$(nproc)
sudo make install
```

---

🧩 Task 7 — Build and Install RISC‑V Proxy Kernel (pk)

```bash
cd $pwd/riscv_toolchain
git clone https://github.com/riscv/riscv-pk.git
cd riscv-pk
mkdir -p build && cd build
../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 --host=riscv64-unknown-elf
make -j$(nproc)
sudo make install
```

---

🛤️ Task 8 — Ensure pk is in PATH

```bash
echo 'export PATH=$HOME/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

🔬 Task 9 — (Optional) Install Icarus Verilog

```bash
cd $pwd/riscv_toolchain
git clone https://github.com/steveicarus/iverilog.git
cd iverilog
git checkout --track -b v10-branch origin/v10-branch
git pull
chmod +x autoconf.sh
./autoconf.sh
./configure
make -j$(nproc)
sudo make install
```

---

✅ Task 10 — Sanity Checks

```bash
which riscv64-unknown-elf-gcc
riscv64-unknown-elf-gcc -v

which spike
spike --version || spike -h

which pk
```

---

🔑 **Final Deliverable — Unique C Test**

📄 **unique\_test.c**

```c
#include <stdint.h>
#include <stdio.h>

#ifndef USERNAME
#define USERNAME "sharon-hp"
#endif
#ifndef HOSTNAME
#define HOSTNAME "sharonhp-VirtualBox"
#endif

static uint64_t fnv1a64(const char *s) {
    const uint64_t FNV_OFFSET = 1469598103934665603ULL;
    const uint64_t FNV_PRIME  = 1099511628211ULL;
    uint64_t h = FNV_OFFSET;
    for (const unsigned char *p = (const unsigned char*)s; *p; ++p) {
        h ^= (uint64_t)(*p);
        h *= FNV_PRIME;
    }
    return h;
}

int main(void) {
    const char *user = USERNAME;
    const char *host = HOSTNAME;

    char buf[256];
    int n = snprintf(buf, sizeof(buf), "%s@%s", user, host);
    if (n <= 0) return 1;

    uint64_t uid = fnv1a64(buf);

    printf("RISC-V Uniqueness Check\n");
    printf("User: %s\n", user);
    printf("Host: %s\n", host);
    printf("UniqueID: 0x%016llx\n", (unsigned long long)uid);

#ifdef __VERSION__
    unsigned long long vlen = (unsigned long long)sizeof(__VERSION__) - 1;
    printf("GCC_VLEN: %llu\n", vlen);
#endif
    return 0;
}
```

⚙️ **Compile Command Used**

```bash
riscv64-unknown-elf-gcc -O2 -Wall -march=rv64imac -mabi=lp64 \
  -DUSERNAME="\"$(id -un)\"" -DHOSTNAME="\"$(hostname -s)\"" \
  unique_test.c -o unique_test

```

▶️ **Run on Spike**

```bash
spike ~/riscv_toolchain/riscv-pk/build/pk ./unique_test
```

🖥️ **Output**

```
bbl loader
RISC-V Uniqueness Check
User: sharon-hp
Host: sharonhp-VirtualBox
UniqueID: 0xf8f1bc67a91bb4c1
GCC_VLEN: 5
```

---





📸 Implementation Output
<img width="955" height="474" alt="Screenshot from 2025-08-01 18-10-35" src="https://github.com/user-attachments/assets/9841acc0-2949-4dc9-9497-fbe61eaba6c4" />
<img width="1272" height="788" alt="image" src="https://github.com/user-attachments/assets/ac9491f3-86f3-4603-ac90-82818fdd0246" />

<img width="1210" height="773" alt="image" src="https://github.com/user-attachments/assets/a4f78687-2455-4554-92c9-164e2d8bbb59" />


🛠️ 🏗️Task 2 — Local RISCV Setup (Run, Disassemble,Decode)
---

🛠️ Step 1 — Compile All Programs with Uniqueness Macros

**factorial.c**
```bash
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
factorial.c -o factorial
```

**max_array.c**
```bash
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
max_array.c -o max_array
```

**bitops.c**
```bash
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
bitops.c -o bitops
```

**bubble_sort.c**
```bash
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
bubble_sort.c -o bubble_sort
```

---

🖥️ Step 2 — Run on Spike

```bash
spike ~/riscv_toolchain/riscv-pk/build/pk ./factorial
spike ~/riscv_toolchain/riscv-pk/build/pk ./max_array
spike ~/riscv_toolchain/riscv-pk/build/pk ./bitops
spike ~/riscv_toolchain/riscv-pk/build/pk ./bubble_sort
```

📂 Step 3 — Generate Assembly & Disassembly

**Assembly:**
```bash
riscv64-unknown-elf-gcc -O0 -S file.c -o file.s
```

**Disassembly (`main` only):**
```bash
riscv64-unknown-elf-objdump -d ./file | sed -n '/<main>:/,/^$/p' > file_main_objdump.txt
```

Repeat for all four `.c` files.

---

📊 Step 4— Instruction Decoding

| Assembly Instruction | Hex Source | Opcode  | rd       | rs1      | rs2/imm   | funct3 | funct7   | 32-bit Binary Representation            | Description                                     |
|----------------------|------------|---------|----------|----------|-----------|--------|----------|-------------------------------------------|-------------------------------------------------|
| `addi sp,sp,-32`     | 0x1101     | 0010011 | x2 (sp)  | x2 (sp)  | -32       | 000    | —        | 111111111000 00010 000 00010 0010011       | sp = sp + (-32)                                 |
| `sd ra,24(sp)`       | 0xec06     | 0100011 | —        | x2 (sp)  | x1 (ra)   | 011    | —        | 0000000 00001 00010 011 11000 0100011      | mem[sp+24] = ra                                 |
| `sd s0,16(sp)`       | 0xe822     | 0100011 | —        | x2 (sp)  | x8 (s0)   | 011    | —        | 0000000 01000 00010 011 10000 0100011      | mem[sp+16] = s0                                 |
| `addi s0,sp,32`      | 0x1000     | 0010011 | x8 (s0)  | x2 (sp)  | 32        | 000    | —        | 00000000100000010 000 01000 0010011        | s0 = sp + 32                                    |
| `lui a5,0x1c`        | 0x67f1     | 0110111 | x15 (a5) | —        | 0x1c000   | —      | —        | 0000000000011100 01111 0110111             | a5 = 0x1c << 12                                 |

---
