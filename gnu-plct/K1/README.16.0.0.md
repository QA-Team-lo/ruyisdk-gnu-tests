---
title: "SpacemiT K1/M1 (X60) GNU Toolchain (gnu-plct) Test Report"
target_config: "targets/k1.toml"
unit_name: "gnu-plct"
unit_version: "0.1.0" # the version of this test unit itself
tags: ["toolchain", "gcc", "gnu-plct", "K1"]
gcc_name: "riscv64-plct-linux-gnu-gcc"
---

# SpacemiT K1/M1 (X60) GNU Toolchain (gnu-plct) Test Report

This report details a series of basic functionality and performance tests conducted on the bpi-f3 development board using the PLCT GNU Toolchain (`gnu-plct`) provided by RuyiSDK. The tests aim to verify the correct installation of the toolchain, its basic compilation and linking capabilities, and its performance on a standard benchmark (CoreMark).

Tested by lintestor 0.2.0-rc. Refer to [gnu-plct/K1](https://github.com/QA-Team-lo/ruyisdk-gnu-tests/blob/lintestor-templates/gnu-plct/K1/README.test.md) to see the test template.

## Environment Information

The following hardware and software environment was used for this test:

### System Information

* **Test Date:** `2025-07-04`
* **Target Configuration:** `target/k1.toml`
* **Test Unit Name:** `gnu-plct`
* **Test Unit Version:** `0.1.0`
* **GCC Version (from -v):** `16.0.0`
* RuyiSDK running on a `BananaPI F3`.

### Hardware Information

* Banana Pi BPI-F3 board
* SpacemiT K1/M1 SoC (RISC-V SpacemiT X60 core)

## Installation

This section documents the process of installing and setting up the PLCT GNU Toolchain using RuyiSDK.

### 0. Initialize Ruyi Package Manager

Clean up any potentially existing old environments and download/prepare the Ruyi CLI tool.

```bash
rm -rf ~/venv-gnu-plct ~/ruyi /tmp/coremark_* #~/.local/share/ruyi/
curl -Lo ~/ruyi https://mirror.iscas.ac.cn/ruyisdk/ruyi/tags/0.36.0/ruyi.riscv64
chmod +x ~/ruyi
echo "Ruyi CLI downloaded and prepared. Old directories cleaned."
~/ruyi update
```

**Command Output:**

```output {ref="init-ruyi"}
[stdout]
Ruyi CLI downloaded and prepared. Old directories cleaned.

There are 34 new news item(s):

 No. ID Title
────────────────────────────────────────────────────────────────────────────────
 1 2024-01-14-ruyi-news RuyiSDK 支持展示新闻了
 2 2024-01-15-new-board-images 新增板卡支持 (2024-01-15)
 3 2024-01-29-new-board-images 新增板卡支持 (2024-01-29)
 4 2024-01-29-ruyi-0.4 RuyiSDK 0.4 版本更新说明
 5 2024-02-26-gnu-plct-rv64ilp32-elf RV64ILP32 裸机工具链与 profile
 现已可用
 6 2024-04-23-ruyi-0.9 RuyiSDK 0.9 版本更新说明
 7 2024-05-14-ruyi-0.10 RuyiSDK 0.10 版本更新说明
 8 2024-05-28-ruyi-0.11 RuyiSDK 0.11 版本更新说明
 9 2024-06-11-ruyi-0.12 RuyiSDK 0.12 版本更新说明
 10 2024-06-24-ruyi-0.13 RuyiSDK 0.13 版本更新说明
 11 2024-07-08-box64-wps-office-poc 尝鲜：使用 Box64 在 RISC-V
 系统上运行 WPS Office
 12 2024-07-09-ruyi-0.14 RuyiSDK 0.14 版本更新说明
 13 2024-07-23-ruyi-0.15 RuyiSDK 0.15 版本更新说明
 14 2024-08-13-ruyi-0.16 RuyiSDK 0.16 版本更新说明
 15 2024-09-03-ruyi-0.17 RuyiSDK 0.17 版本更新说明
 16 2024-09-14-ruyi-0.18 RuyiSDK 0.18 版本更新说明
 17 2024-09-30-ruyi-0.19 RuyiSDK 0.19 版本更新说明
 18 2024-10-22-ruyi-0.20 RuyiSDK 0.20 版本更新说明
 19 2024-11-05-ruyi-0.21 RuyiSDK 0.21 版本更新说明
 20 2024-11-19-ruyi-0.22 RuyiSDK 0.22 版本更新说明
 21 2024-12-03-ruyi-0.23 RuyiSDK 0.23 版本更新说明
 22 2024-12-17-ruyi-0.24 RuyiSDK 0.24 版本更新说明
 23 2024-12-31-ruyi-0.25 RuyiSDK 0.25 版本更新说明
 24 2025-01-14-ruyi-0.26 RuyiSDK 0.26 版本更新说明
 25 2025-02-11-ruyi-0.27 RuyiSDK 0.27 版本更新说明
 26 2025-02-25-ruyi-0.28 RuyiSDK 0.28 版本更新说明
 27 2025-03-11-ruyi-0.29 RuyiSDK 0.29 版本更新说明
 28 2025-03-25-ruyi-0.30 RuyiSDK 0.30 版本更新说明
 29 2025-04-08-ruyi-0.31 RuyiSDK 0.31 版本更新说明
 30 2025-04-22-ruyi-0.32 RuyiSDK 0.32 版本更新说明
 31 2025-05-13-ruyi-0.33 RuyiSDK 0.33 版本更新说明
 32 2025-05-27-ruyi-0.34 RuyiSDK 0.34 版本更新说明
 33 2025-06-10-ruyi-0.35 RuyiSDK 0.35 版本更新说明
 34 2025-06-24-ruyi-0.36 RuyiSDK 0.36 版本更新说明

You can read them with ruyi news read.
[stderr]
 % Total % Received % Xferd Average Speed Time Time Time Current
 Dload Upload Total Spent Left Speed

 0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0
 0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0
100 142 100 142 0 0 213 0 --:--:-- --:--:-- --:--:-- 213

 26 24.0M 26 6479k 0 0 4878k 0 0:00:05 0:00:01 0:00:04 4878k
100 24.0M 100 24.0M 0 0 14.1M 0 0:00:01 0:00:01 --:--:-- 47.7M
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-07-09 08:00:00 +0800 and 2025-07-10 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
```

Ruyi CLI Initialization Status (based on `assert.exit_code`): Pass

### 1. Install Toolchain

Install the PLCT GNU Toolchain package using Ruyi.

```bash
~/ruyi install toolchain/gnu-plct
```

**Command Output:**

```output {ref="install-toolchain"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-07-09 08:00:00 +0800 and 2025-07-10 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: skipping already installed package gnu-plct-0.20250615.0
```

This command downloaded the toolchain package from the configured mirror and installed it into Ruyi's local repository.

Installation Status (based on `assert.exit_code`): Pass

### 2. Create Virtual Environment

Create an isolated virtual environment for this test.

```bash
~/ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Command Output:**

```output {ref="create-venv"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-07-09 08:00:00 +0800 and 2025-07-10 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-plct...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

 /home/ezra/venv-gnu-plct/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

This step created a virtual environment directory at `venv-gnu-plct`.
Creation Status (based on `assert.exit_code`): Pass

**Verify Created Environment Contents:**

```bash
ls ~/venv-gnu-plct/
ls ~/venv-gnu-plct/bin/
```

**Command Output:**

```output {ref="verify-venv"}
[stdout]
bin
meson-cross.ini
meson-cross.riscv64-plct-linux-gnu.ini
ruyi-cache.v2.toml
ruyi-venv.toml
sysroot
sysroot.riscv64-plct-linux-gnu
toolchain.cmake
toolchain.riscv64-plct-linux-gnu.cmake
riscv64-plct-linux-gnu-addr2line
riscv64-plct-linux-gnu-ar
riscv64-plct-linux-gnu-as
riscv64-plct-linux-gnu-c++
riscv64-plct-linux-gnu-cc
riscv64-plct-linux-gnu-c++filt
riscv64-plct-linux-gnu-cpp
riscv64-plct-linux-gnu-elfedit
riscv64-plct-linux-gnu-g++
riscv64-plct-linux-gnu-gcc
riscv64-plct-linux-gnu-gcc-ar
riscv64-plct-linux-gnu-gcc-nm
riscv64-plct-linux-gnu-gcc-ranlib
riscv64-plct-linux-gnu-gcov
riscv64-plct-linux-gnu-gcov-dump
riscv64-plct-linux-gnu-gcov-tool
riscv64-plct-linux-gnu-gdb
riscv64-plct-linux-gnu-gdb-add-index
riscv64-plct-linux-gnu-gfortran
riscv64-plct-linux-gnu-gp-archive
riscv64-plct-linux-gnu-gp-collect-app
riscv64-plct-linux-gnu-gp-display-html
riscv64-plct-linux-gnu-gp-display-src
riscv64-plct-linux-gnu-gp-display-text
riscv64-plct-linux-gnu-gprof
riscv64-plct-linux-gnu-gprofng
riscv64-plct-linux-gnu-gprofng-archive
riscv64-plct-linux-gnu-gprofng-collect-app
riscv64-plct-linux-gnu-gprofng-display-html
riscv64-plct-linux-gnu-gprofng-display-src
riscv64-plct-linux-gnu-gprofng-display-text
riscv64-plct-linux-gnu-gstack
riscv64-plct-linux-gnu-ld
riscv64-plct-linux-gnu-ld.bfd
riscv64-plct-linux-gnu-ldd
riscv64-plct-linux-gnu-lto-dump
riscv64-plct-linux-gnu-nm
riscv64-plct-linux-gnu-objcopy
riscv64-plct-linux-gnu-objdump
riscv64-plct-linux-gnu-ranlib
riscv64-plct-linux-gnu-readelf
riscv64-plct-linux-gnu-size
riscv64-plct-linux-gnu-strings
riscv64-plct-linux-gnu-strip
ruyi-activate
```

Environment Contents Verification (based on `assert.stdout_contains`):
- `assert.exit_code=0`: Pass
- `bin` `ruyi-venv.toml` `toolchain.cmake` `riscv64-plct-linux-gnu-gcc` `ruyi-activate` exists: Pass

### 3. Activate Environment

Activate the virtual environment.

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
echo "Environment activated. Current PATH: $PATH"
```

**Command Output:**

```output {ref="activate-venv"}
[stdout]
Environment activated. Current PATH: /home/ezra/venv-gnu-plct/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

**Command Output (Activate Environment):**

```output {ref="activate-venv"}
[stdout]
Environment activated. Current PATH: /home/ezra/venv-gnu-plct/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

Environment Activation Status (based on `assert.exit_code` and `assert.stdout_contains`): Success

## Tests & Results

### 1. Compiler Version Check

Check the version information of `riscv64-plct-linux-gnu-gcc`.

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
riscv64-plct-linux-gnu-gcc -v 2>&1
```

**Command Output:**

```output {ref="check-version"}
[stdout]
Using built-in specs.
COLLECT_GCC=/home/ezra/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/ezra/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/16.0.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250615 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --enable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 16.0.0 20250512 (experimental) (RuyiSDK 20250615 PLCT-Sources)
```

**Analysis:**
Exit code check: Pass
GCC version check (`assert.stdout_contains="gcc version"`): Pass
The output shows detailed GCC version and configuration options:
* GCC Version: `16.0.0`
* Target Triple: `riscv64-plct-linux-gnu` (Does not match expected riscv64-unknown-linux-gnu)
* Configured Arch: `rv64gc` (Matches expected rv64gc)
* Configured ABI: `lp64d` (Matches expected lp64d)

### 2. Hello World Program

Compile and run a simple "Hello, world!" C program.

```bash
cd /tmp
cat > hello.c << 'EOF'
#include <stdio.h>

int main() {printf("Hello, world!\n"); return 0;}
EOF
```

**Command Output (Create source file):**

```output {ref="create-hello"}
```

Source file `hello.c` creation status (based on `assert.exit_code`): Pass

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
cd /tmp
riscv64-plct-linux-gnu-gcc hello.c -o hello_plct
file hello_plct
```

**Command Output (Compile and check ELF format):**

```output {ref="compile-hello"}
[stdout]
hello_plct: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=d0a47dcba1edcf817d618baa447ec0ab1989fdf7, for GNU/Linux 4.15.0, with debug_info, not stripped
```

**Analysis:**
Compilation and linking status (based on `assert.exit_code`): Pass.
ELF format checks (based on `assert.stdout_contains`):
- "ELF 64-bit LSB executable", "RISC-V", "dynamically linked": Pass
Generated `hello_plct` file type: `ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1`.
Interpreter: `/lib/ld-linux-riscv64-lp64d.so.1` (Matches expected).

```bash
cd /tmp
./hello_plct
```

**Command Output (Run program):**

```output {ref="run-hello"}
[stdout]
Hello, world!
```

**Analysis:**
Program execution status (based on `assert.exit_code`): Pass.
Program output: `Hello, world!`.
Output correctness (based on `assert.stdout`): Correct (Output matches 'Hello, world!').

### 3. CoreMark Benchmark (Default Optimizations)

Compile and run CoreMark using default optimization options (`-O2 -lrt`).

**Command (Extract CoreMark - Default):**

```bash
mkdir -p /tmp/coremark_default
cd /tmp/coremark_default
~/ruyi extract coremark
```

**Command Output (Extract CoreMark - Default):**

```output {ref="extract-coremark-default"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-07-09 08:00:00 +0800 and 2025-07-10 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

CoreMark (Default) extraction status (based on `assert.exit_code`): Pass.

**Command (Configure Makefile - Default):**

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
cd /tmp/coremark_default
sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

**Command Output (Configure Makefile - Default):**

```output {ref="config-coremark-default"}
```

Makefile (Default) configuration status (based on `assert.exit_code`): Pass.

**Command (Compile CoreMark - Default Optimizations):**

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
cd /tmp/coremark_default
make PORT_DIR=linux64 link
file coremark.exe
```

**Command Output (Compile CoreMark - Default Optimizations):**

```output {ref="build-coremark-default"}
[stdout]
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -lrt"\" -DITERATIONS=0 core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=865e58d260b2b3190200b6f5d1e22ca967733977, for GNU/Linux 4.15.0, with debug_info, not stripped
```

CoreMark (Default Opt.) compilation status (based on `assert.exit_code` and `assert.stdout_contains`): ✅ PASS.

**Command (Run CoreMark - Default Optimizations):**

```bash
cd /tmp/coremark_default
./coremark.exe
```

**Command Output (Run CoreMark - Default Optimizations):**

```output {ref="run-coremark-default"}
[stdout]
2K performance run parameters for coremark.
CoreMark Size : 666
Total ticks : 18979
Total time (secs): 18.979000
Iterations/Sec : 5795.879656
Iterations : 110000
Compiler version : GCC16.0.0 20250512 (experimental)
Compiler flags : -O2 -lrt
Memory location : Please put data memory location here
 (e.g. code in flash, data on heap etc)
seedcrc : 0xe9f5
[0]crclist : 0xe714
[0]crcmatrix : 0x1fd7
[0]crcstate : 0x8e3a
[0]crcfinal : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5795.879656 / GCC16.0.0 20250512 (experimental) -O2 -lrt / Heap
```

**Results (Default Optimizations):**
CoreMark run status (based on `assert.stdout_contains="CoreMark 1.0"`): Pass
CoreMark Score (Iterations/Sec): `5795.879656`
Reported Compiler Version: `GCC16.0.0` (Does not match -v or not extracted)
Reported Compiler Flags: `-O2 -lrt`

### 4. CoreMark Benchmark (Vector Extension Optimizations)

Compile and run CoreMark using `-march=rv64gcv_zvl256b -mabi=lp64d`.

**Command (Extract CoreMark - Vector):**

```bash
mkdir -p /tmp/coremark_vector
cd /tmp/coremark_vector
~/ruyi extract coremark
```

**Command Output (Extract CoreMark - Vector):**

```output {ref="extract-coremark-vector"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-07-09 08:00:00 +0800 and 2025-07-10 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

CoreMark (Vector) extraction status (based on `assert.exit_code`): Pass.

**Command (Configure Makefile - Vector):**

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
cd /tmp/coremark_vector
sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

**Command Output (Configure Makefile - Vector):**

```output {ref="config-coremark-vector"}
```

Makefile (Vector) configuration status (based on `assert.exit_code`): Pass.

**Command (Compile CoreMark - Vector Optimizations):**

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
cd /tmp/coremark_vector
make PORT_DIR=linux64 XCFLAGS="-march=rv64gcv_zvl256b -mabi=lp64d" link
file coremark.exe
```

**Command Output (Compile CoreMark - Vector Optimizations):**

```output {ref="build-coremark-vector"}
[stdout]
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -march=rv64gcv_zvl256b -mabi=lp64d -lrt"\" -DITERATIONS=0 -march=rv64gcv_zvl256b -mabi=lp64d core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=3c68395bf758c26aefe0070688e403ceb3bee4ea, for GNU/Linux 4.15.0, with debug_info, not stripped
```

CoreMark (Vector Opt.) compilation status (based on `assert.exit_code` and `assert.stdout_contains`): ✅ PASS.

**Command (Run CoreMark - Vector Optimizations):**

```bash
cd /tmp/coremark_vector
./coremark.exe
```

**Command Output (Run CoreMark - Vector Optimizations):**

```output {ref="run-coremark-vector"}
[stdout]
2K performance run parameters for coremark.
CoreMark Size : 666
Total ticks : 19067
Total time (secs): 19.067000
Iterations/Sec : 5769.129910
Iterations : 110000
Compiler version : GCC16.0.0 20250512 (experimental)
Compiler flags : -O2 -march=rv64gcv_zvl256b -mabi=lp64d -lrt
Memory location : Please put data memory location here
 (e.g. code in flash, data on heap etc)
seedcrc : 0xe9f5
[0]crclist : 0xe714
[0]crcmatrix : 0x1fd7
[0]crcstate : 0x8e3a
[0]crcfinal : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5769.129910 / GCC16.0.0 20250512 (experimental) -O2 -march=rv64gcv_zvl256b -mabi=lp64d -lrt / Heap
```

**Results (Vector Optimizations):**
CoreMark run status (based on `assert.stdout_contains="CoreMark 1.0"`): Pass
CoreMark Score (Iterations/Sec): `5769.129910`
Reported Compiler Version: `GCC16.0.0`
Reported Compiler Flags: `-O2 -march=rv64gcv_zvl256b -mabi=lp64d -lrt`

## Performance Comparison

Higher CoreMark scores (Iterations/Sec) indicate better performance.

| Metric | Default Optimizations | Vector Extension Optimizations|
|-------------------------------|-----------------------------------------------------|-----------------------------------------------------------------------|
| **Iterations/Sec** | `5795.879656` | `5769.129910` |
| **Total ticks** | `18979` | `19067` |
| **Total time (secs)** | `18.979000` | `19.067000` |
| **Iterations** | `110000` | `110000` |
| **Compiler (Reported)** | `GCC16.0.0` | `GCC16.0.0` |
| **Compiler Flags (Reported)** | `-O2 -lrt` | `-O2 -march=rv64gcv_zvl256b -mabi=lp64d -lrt` |
| **Compiler (from -v)** | `16.0.0` | `16.0.0` |

**Performance Analysis:**
In this test, CoreMark on the SpacemiT K1/M1 (X60) SoC using GCC `16.0.0` achieved scores of `5795.879656` (Default Optimizations) and `5769.129910` (Vector Extension Optimizations).
Further analysis of these scores can reveal the specific impact of vector extensions for the CoreMark workload with this compiler version.

## Test Summary

| Test Item | Expected Result | Actual Status (template logic) |
|---------------------------------------------------|-------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ruyi CLI Initialization (`init-ruyi`) | Ruyi CLI successfully prepared | ✅ PASS |
| Toolchain Installation (`install-toolchain`) | Successfully installed | ✅ PASS |
| Compiler Version Check (`check-version`) | GCC , riscv64-unknown-linux-gnu, rv64gc, lp64d | ✅ PASS (Version: `16.0.0`) |
| Hello World Compilation (`compile-hello`) | Successfully compiled ELF 64-bit | ✅ PASS |
| Hello World Execution (`run-hello`) | Outputs "Hello, world!" | ✅ PASS (Output: `Hello, world!`) |
| CoreMark (Default) Compilation (`build-coremark-default`) | Successfully compiled | ✅ PASS |
| CoreMark (Default) Execution (`run-coremark-default`) | Successfully runs, "CoreMark 1.0" flag present | ✅ PASS (Score: `5795.879656`) |
| CoreMark (Vector) Compilation (`build-coremark-vector`) | Successfully compiled | ✅ PASS |
| CoreMark (Vector) Execution (`run-coremark-vector`) | Successfully runs, "CoreMark 1.0" flag present | ✅ PASS (Score: `5769.129910`) |
