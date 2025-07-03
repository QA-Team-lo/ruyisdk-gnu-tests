---
title: "Lichee Pi 4A GNU Toolchain (gnu-plct) Test Report"
target_config: "targets/lpi4a.toml"
unit_name: "gnu-plct"
unit_version: "0.1.0" # the version of this test unit itself
tags: ["toolchain", "gcc", "gnu-plct", "TH1520"]
gcc_name: "riscv64-plct-linux-gnu-gcc"
---

# Lichee Pi 4A GNU Toolchain (gnu-plct) Test Report

This report details a series of basic functionality and performance tests conducted on the lpi-4a development board using the PLCT GNU Toolchain (`gnu-plct`) provided by RuyiSDK. The tests aim to verify the correct installation of the toolchain, its basic compilation and linking capabilities, and its performance on a standard benchmark (CoreMark).

## Environment Information

The following hardware and software environment was used for this test:

### System Information

* **Test Date:** `2025-07-03`
* **Target Configuration:** `targets/lpi4a.toml`
* **Test Unit Name:** `gnu-plct`
* **Test Unit Version:** `0.1.0`
* **GCC Version (from -v):** `16.0.0`
* RuyiSDK running on a `LicheePi 4A`.

### Hardware Information

* Lichee Pi 4A board
* TH1520 SoC

## Installation

This section documents the process of installing and setting up the PLCT GNU Toolchain using RuyiSDK.

### 0. Initialize Ruyi Package Manager

Clean up any potentially existing old environments and download/prepare the Ruyi CLI tool.

```bash
rm -rf ~/venv-gnu-plct ~/ruyi /tmp/coremark_* #~/.local/share/ruyi/
#curl -Lo ~/ruyi https://mirror.iscas.ac.cn/ruyisdk/ruyi/tags/0.33.0/ruyi.riscv64
#chmod +x ~/ruyi
echo "We assume our LPi4A got ruyi in PATH, only cleaned ruyi dists."
ruyi update
```

**Command Output:**

```output {ref="init-ruyi"}
[stdout]
We assume our LPi4A got ruyi in PATH, only cleaned ruyi dists.
Enumerating objects: 57, done.
Counting objects: 1% (1/57)Counting objects: 3% (2/57)Counting objects: 5% (3/57)Counting objects: 7% (4/57)Counting objects: 8% (5/57)Counting objects: 10% (6/57)Counting objects: 12% (7/57)Counting objects: 14% (8/57)Counting objects: 15% (9/57)Counting objects: 17% (10/57)Counting objects: 19% (11/57)Counting objects: 21% (12/57)Counting objects:
22% (13/57)Counting objects: 24% (14/57)Counting objects: 26% (15/57)Counting objects: 28% (16/57)Counting objects: 29% (17/57)Counting objects:
31% (18/57)Counting objects: 33% (19/57)Counting objects: 35% (20/57)Counting objects: 36% (21/57)Counting objects: 38% (22/57)Counting objects:
40% (23/57)Counting objects: 42% (24/57)Counting objects: 43% (25/57)Counting objects:
45% (26/57)Counting objects: 47% (27/57)Counting objects: 49% (28/57)Counting objects: 50% (29/57)Counting objects: 52% (30/57)Counting objects: 54% (31/57)Counting objects: 56% (32/57)Counting objects:
57% (33/57)Counting objects: 59% (34/57)Counting objects: 61% (35/57)Counting objects:
63% (36/57)Counting objects: 64% (37/57)Counting objects: 66% (38/57)Counting objects:
68% (39/57)Counting objects: 70% (40/57)Counting objects: 71% (41/57)Counting objects: 73% (42/57)Counting objects: 75% (43/57)Counting objects:
77% (44/57)Counting objects: 78% (45/57)Counting objects: 80% (46/57)Counting objects: 82% (47/57)Counting objects: 84% (48/57)Counting objects:
85% (49/57)Counting objects: 87% (50/57)Counting objects: 89% (51/57)Counting objects: 91% (52/57)Counting objects: 92% (53/57)Counting objects: 94% (54/57)Counting objects: 96% (55/57)Counting objects: 98% (56/57)Counting objects: 100% (57/57)Counting objects: 100% (57/57), done.
Compressing objects: 4% (1/22)Compressing objects: 9% (2/22)Compressing objects: 13% (3/22)Compressing
objects: 18% (4/22)Compressing objects: 22% (5/22)Compressing objects: 27% (6/22)Compressing objects: 31% (7/22)Compressing objects: 36% (8/22)Compressing objects: 40% (9/22)Compressing objects: 45% (10/22)Compressing objects: 50% (11/22)Compressing objects: 54% (12/22)Compressing objects: 59% (13/22)Compressing objects: 63% (14/22)Compressing objects: 68% (15/22)Compressing objects: 72% (16/22)Compressing objects: 77% (17/22)Compressing objects: 81% (18/22)Compressing objects: 86% (19/22)Compressing objects: 90% (20/22)Compressing objects: 95% (21/22)Compressing objects: 100% (22/22)Compressing objects: 100% (22/22), done.
Total 44 (delta 28), reused 34 (delta 20), pack-reused 0 (from 0)
transferring objects ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 0% -:--:--
processing deltas ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00

There are 34 new news item(s):

 No. ID Title
────────────────────────────────────────────────────────────────────────────────
 1 2024-01-14-ruyi-news RuyiSDK now supports displaying news
 2 2024-01-15-new-board-images New board images available
 (2024-01-15)
 3 2024-01-29-new-board-images New board images available
 (2024-01-29)
 4 2024-01-29-ruyi-0.4 Release notes for RuyiSDK 0.4
 5 2024-02-26-gnu-plct-rv64ilp32-elf RV64ILP32 bare-metal toolchain &
 profile now available
 6 2024-04-23-ruyi-0.9 Release notes for RuyiSDK 0.9
 7 2024-05-14-ruyi-0.10 Release notes for RuyiSDK 0.10
 8 2024-05-28-ruyi-0.11 Release notes for RuyiSDK 0.11
 9 2024-06-11-ruyi-0.12 Release notes for RuyiSDK 0.12
 10 2024-06-24-ruyi-0.13 Release notes for RuyiSDK 0.13
 11 2024-07-08-box64-wps-office-poc 尝鲜：使用 Box64 在 RISC-V
 系统上运行 WPS Office
 12 2024-07-09-ruyi-0.14 Release notes for RuyiSDK 0.14
 13 2024-07-23-ruyi-0.15 Release notes for RuyiSDK 0.15
 14 2024-08-13-ruyi-0.16 Release notes for RuyiSDK 0.16
 15 2024-09-03-ruyi-0.17 Release notes for RuyiSDK 0.17
 16 2024-09-14-ruyi-0.18 Release notes for RuyiSDK 0.18
 17 2024-09-30-ruyi-0.19 Release notes for RuyiSDK 0.19
 18 2024-10-22-ruyi-0.20 Release notes for RuyiSDK 0.20
 19 2024-11-05-ruyi-0.21 Release notes for RuyiSDK 0.21
 20 2024-11-19-ruyi-0.22 Release notes for RuyiSDK 0.22
 21 2024-12-03-ruyi-0.23 Release notes for RuyiSDK 0.23
 22 2024-12-17-ruyi-0.24 Release notes for RuyiSDK 0.24
 23 2024-12-31-ruyi-0.25 Release notes for RuyiSDK 0.25
 24 2025-01-14-ruyi-0.26 Release notes for RuyiSDK 0.26
 25 2025-02-11-ruyi-0.27 Release notes for RuyiSDK 0.27
 26 2025-02-25-ruyi-0.28 Release notes for RuyiSDK 0.28
 27 2025-03-11-ruyi-0.29 Release notes for RuyiSDK 0.29
 28 2025-03-25-ruyi-0.30 Release notes for RuyiSDK 0.30
 29 2025-04-08-ruyi-0.31 Release notes for RuyiSDK 0.31
 30 2025-04-22-ruyi-0.32 Release notes for RuyiSDK 0.32
 31 2025-05-13-ruyi-0.33 Release notes for RuyiSDK 0.33
 32 2025-05-27-ruyi-0.34 Release notes for RuyiSDK 0.34
 33 2025-06-10-ruyi-0.35 Release notes for RuyiSDK 0.35
 34 2025-06-24-ruyi-0.36 Release notes for RuyiSDK 0.36

You can read them with ruyi news read.
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: scope pm: usage information has already been uploaded today at 2025-07-03 16:28:30 +0800
info: scope repo:ruyisdk: the next upload will happen today if not already
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
```

Ruyi CLI Initialization Status (based on `assert.exit_code`): Pass

### 1. Install Toolchain

Install the PLCT GNU Toolchain package using Ruyi.

```bash
ruyi install toolchain/gnu-plct
```

**Command Output:**

```output {ref="install-toolchain"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: scope pm: usage information has already been uploaded today at 2025-07-03 16:28:30 +0800
info: scope repo:ruyisdk: the next upload will happen today if not already
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
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Command Output:**

```output {ref="create-venv"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: scope pm: usage information has already been uploaded today at 2025-07-03 16:28:30 +0800
info: scope repo:ruyisdk: the next upload will happen today if not already
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
Environment activated. Current PATH: /home/ezra/venv-gnu-plct/bin:/usr/local/bin:/usr/bin:/bin:/usr/games
```

**Command Output (Activate Environment):**

```output {ref="activate-venv"}
[stdout]
Environment activated. Current PATH: /home/ezra/venv-gnu-plct/bin:/usr/local/bin:/usr/bin:/bin:/usr/games
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
[stderr]
bash: line 1: export: `-mabi=lp64d link
file': not a valid identifier
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
ruyi extract coremark
```

**Command Output (Extract CoreMark - Default):**

```output {ref="extract-coremark-default"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: scope pm: usage information has already been uploaded today at 2025-07-03 16:28:30 +0800
info: scope repo:ruyisdk: the next upload will happen today if not already
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
[stderr]
bash: line 1: export: `-mabi=lp64d link
file': not a valid identifier
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
Total ticks : 13177
Total time (secs): 13.177000
Iterations/Sec : 8347.878880
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
CoreMark 1.0 : 8347.878880 / GCC16.0.0 20250512 (experimental) -O2 -lrt / Heap
```

**Results (Default Optimizations):**
CoreMark run status (based on `assert.stdout_contains="CoreMark 1.0"`): Pass
CoreMark Score (Iterations/Sec): `8347.878880`
Reported Compiler Version: `GCC16.0.0` (Does not match -v or not extracted)
Reported Compiler Flags: `-O2 -lrt`

### 4. CoreMark Benchmark (Vector Extension Optimizations)

Compile and run CoreMark using `-march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d`.

**Command (Extract CoreMark - Vector):**

```bash
mkdir -p /tmp/coremark_vector
cd /tmp/coremark_vector
ruyi extract coremark
```

**Command Output (Extract CoreMark - Vector):**

```output {ref="extract-coremark-vector"}
[stderr]
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: scope pm: usage information has already been uploaded today at 2025-07-03 16:28:30 +0800
info: scope repo:ruyisdk: the next upload will happen today if not already
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
make PORT_DIR=linux64 XCFLAGS="-march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d" link
file coremark.exe
```

**Command Output (Compile CoreMark - Vector Optimizations):**

```output {ref="build-coremark-vector"}
[stdout]
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d -lrt"\" -DITERATIONS=0 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=5091706b6b6b99b8df9c52452e0a40bf7baa26cb, for GNU/Linux 4.15.0, with debug_info, not stripped
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
Total ticks : 12777
Total time (secs): 12.777000
Iterations/Sec : 8609.219692
Iterations : 110000
Compiler version : GCC16.0.0 20250512 (experimental)
Compiler flags : -O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d -lrt
Memory location : Please put data memory location here
 (e.g. code in flash, data on heap etc)
seedcrc : 0xe9f5
[0]crclist : 0xe714
[0]crcmatrix : 0x1fd7
[0]crcstate : 0x8e3a
[0]crcfinal : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 8609.219692 / GCC16.0.0 20250512 (experimental) -O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d -lrt / Heap
```

**Results (Vector Optimizations):**
CoreMark run status (based on `assert.stdout_contains="CoreMark 1.0"`): Pass
CoreMark Score (Iterations/Sec): `8609.219692`
Reported Compiler Version: `GCC16.0.0`
Reported Compiler Flags: `-O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d -lrt`

## Performance Comparison

Higher CoreMark scores (Iterations/Sec) indicate better performance.

| Metric | Default Optimizations | Vector Extension Optimizations|
|-------------------------------|-----------------------------------------------------|-----------------------------------------------------------------------|
| **Iterations/Sec** | `8347.878880` | `8609.219692` |
| **Total ticks** | `13177` | `12777` |
| **Total time (secs)** | `13.177000` | `12.777000` |
| **Iterations** | `110000` | `110000` |
| **Compiler (Reported)** | `GCC16.0.0` | `GCC16.0.0` |
| **Compiler Flags (Reported)** | `-O2 -lrt` | `-O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs -mabi=lp64d -lrt` |
| **Compiler (from -v)** | `16.0.0` | `16.0.0` |

**Performance Analysis:**
In this test, CoreMark on the TH1520 SoC using GCC `16.0.0` achieved scores of `8347.878880` (Default Optimizations) and `8609.219692` (Vector Extension Optimizations).
Further analysis of these scores can reveal the specific impact of vector extensions for the CoreMark workload with this compiler version.

## Test Summary

| Test Item | Expected Result | Actual Status (template logic) |
|---------------------------------------------------|-------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ruyi CLI Initialization (`init-ruyi`) | Ruyi CLI successfully prepared | ✅ PASS |
| Toolchain Installation (`install-toolchain`) | Successfully installed | ✅ PASS |
| Compiler Version Check (`check-version`) | GCC 14.2.0, riscv64-unknown-linux-gnu, rv64gc, lp64d | ✅ PASS (Version: `16.0.0`) |
| Hello World Compilation (`compile-hello`) | Successfully compiled ELF 64-bit | ✅ PASS |
| Hello World Execution (`run-hello`) | Outputs "Hello, world!" | ✅ PASS (Output: `Hello, world!`) |
| CoreMark (Default) Compilation (`build-coremark-default`) | Successfully compiled | ✅ PASS |
| CoreMark (Default) Execution (`run-coremark-default`) | Successfully runs, "CoreMark 1.0" flag present | ✅ PASS (Score: `8347.878880`) |
| CoreMark (Vector) Compilation (`build-coremark-vector`) | Successfully compiled | ✅ PASS |
| CoreMark (Vector) Execution (`run-coremark-vector`) | Successfully runs, "CoreMark 1.0" flag present | ✅ PASS (Score: `8609.219692`) |
