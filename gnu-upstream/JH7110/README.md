# StarFive JH7110 (U74) GNU Toolchain (gnu-upstream) Test Report

## Environment

### System Information

- RuyiSDK on Milk-V Mars with JH7110 (U74) SoC
- OS: Milk-V Mars official Debian image (Releases V1.0.6)
- Testing date: March 16, 2025

### Hardware Information

- Milk-V Mars board (8GB RAM) with Ethernet connection and 64GB SD card
- StarFive JH7110 SoC (SiFive U74 core)

## Installation

### 0. Install ruyi

```bash
sudo apt update
sudo apt install wget xz-utils make
wget https://mirror.iscas.ac.cn/ruyisdk/ruyi/releases/0.28.0/ruyi.riscv64
sudo cp -v ruyi.riscv64 /usr/local/bin/ruyi  # Copy to your $PATH
sudo chmod +x /usr/local/bin/ruyi
```

### 1. Install Toolchain

**Command:**

```bash
ruyi install toolchain/gnu-upstream
```

**Result:**

```bash
user@milkv:~$ ruyi install toolchain/gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-03-20 00:00:00 +0000 and 2025-03-21 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz to /home/user/.cache/ruyi/distfiles/RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz
--2025-03-16 17:27:31--  https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 206 Partial Content
Length: 235308640 (224M), 231853440 (221M) remaining [application/octet-stream]
Saving to: ‘/home/user/.cache/ruyi/distfiles/RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz’

/home/user/.cache/r 100%[===================>] 224.41M  11.0MB/s    in 20s

2025-03-16 17:27:52 (10.9 MB/s) - ‘/home/user/.cache/ruyi/distfiles/RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz’ saved [235308640/235308640]

info: extracting RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz for package gnu-upstream-0.20231212.0
info: package gnu-upstream-0.20231212.0 installed to /home/user/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0
```

This command downloaded the toolchain package (~224MB) from ISCAS mirror and installed it to `/root/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0`.

### 2. Create Virtual Environment

**Command:**

```bash
ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
```

**Result:**

```bash
user@milkv:~$ ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-03-20 00:00:00 +0000 and 2025-03-21 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-upstream...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/user/venv-gnu-upstream/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**

```bash
user@milkv:~$ ls ~/venv-gnu-upstream/
bin
meson-cross.ini
meson-cross.riscv64-unknown-linux-gnu.ini
ruyi-cache.v2.toml
ruyi-venv.toml
sysroot
sysroot.riscv64-unknown-linux-gnu
toolchain.cmake
toolchain.riscv64-unknown-linux-gnu.cmake
user@milkv:~$

user@milkv:~$ ls ~/venv-gnu-upstream/bin/
riscv64-unknown-linux-gnu-addr2line   riscv64-unknown-linux-gnu-gdb-add-index
riscv64-unknown-linux-gnu-ar          riscv64-unknown-linux-gnu-gfortran
riscv64-unknown-linux-gnu-as          riscv64-unknown-linux-gnu-gprof
riscv64-unknown-linux-gnu-c++         riscv64-unknown-linux-gnu-ld
riscv64-unknown-linux-gnu-c++filt     riscv64-unknown-linux-gnu-ld.bfd
riscv64-unknown-linux-gnu-cc          riscv64-unknown-linux-gnu-ldd
riscv64-unknown-linux-gnu-cpp         riscv64-unknown-linux-gnu-lto-dump
riscv64-unknown-linux-gnu-elfedit     riscv64-unknown-linux-gnu-nm
riscv64-unknown-linux-gnu-g++         riscv64-unknown-linux-gnu-objcopy
riscv64-unknown-linux-gnu-gcc         riscv64-unknown-linux-gnu-objdump
riscv64-unknown-linux-gnu-gcc-ar      riscv64-unknown-linux-gnu-ranlib
riscv64-unknown-linux-gnu-gcc-nm      riscv64-unknown-linux-gnu-readelf
riscv64-unknown-linux-gnu-gcc-ranlib  riscv64-unknown-linux-gnu-size
riscv64-unknown-linux-gnu-gcov        riscv64-unknown-linux-gnu-strings
riscv64-unknown-linux-gnu-gcov-dump   riscv64-unknown-linux-gnu-strip
riscv64-unknown-linux-gnu-gcov-tool   ruyi-activate
riscv64-unknown-linux-gnu-gdb
```

This step created a virtual environment at `/root/venv-gnu-upstream/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-unknown-linux-gnu-`.

### 3. Activate Environment

**Command:**

```bash
. ~/venv-gnu-upstream/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:

```bash
user@milkv:~$ . ~/venv-gnu-upstream/bin/ruyi-activate
«Ruyi venv-gnu-upstream» user@milkv:~$
```

This initialized the environment and provided access to all cross-compilation tools.

## Tests & Results

### 1. Compiler Version Check

**Command:**

```bash
riscv64-unknown-linux-gnu-gcc -v
```

**Result:**

```bash
«Ruyi venv-gnu-upstream» user@milkv:~$ riscv64-unknown-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/user/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0/bin/riscv64-unknown-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/user/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0/bin/../libexec/gcc/riscv64-unknown-linux-gnu/13.2.0/lto-wrapper
Target: riscv64-unknown-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-unknown-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20231212 Upstream-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --enable-libquadmath --enable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 13.2.0 (RuyiSDK 20231212 Upstream-Sources)
```

The output confirmed a successful installation showing:

- GCC version: 13.2.0 (RuyiSDK 20231212 Upstream-Sources)
- Target architecture: riscv64-unknown-linux-gnu
- Thread model: posix
- Configured with appropriate RISC-V specific options (rv64gc architecture, lp64d ABI)

### 2. Hello World Program

**Commands:**

```bash
«Ruyi venv-gnu-upstream» user@milkv:~$ cat Hello_Mars.c
#include <stdio.h>

int main(void)
{
    printf("Milk-V Mars: Hello World!\n");
    return 0;
}

«Ruyi venv-gnu-upstream» user@milkv:~$ riscv64-unknown-linux-gnu-gcc Hello_Mars.c -o Hello_Mars_upstream

«Ruyi venv-gnu-upstream» user@milkv:~$ ./Hello_Mars_upstream
Milk-V Mars: Hello World!
```

The program executed successfully and output "Milk-V Mars: Hello World!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:

```bash
«Ruyi venv-gnu-upstream» user@milkv:~$ mkdir coremark_upstream && cd coremark_upstream
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$

«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-03-20 00:00:00 +0000 and 2025-03-21 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ ls
LICENSE.md  barebones         core_matrix.c  coremark.h  linux
Makefile    core_list_join.c  core_state.c   cygwin      linux64
README.md   core_main.c       core_util.c    docs        simple
```

2. Modify the build configuration to use the RISC-V compiler:

```bash
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ cat ./linux64/core_portme.mak | grep "gcc"
CC = riscv64-unknown-linux-gnu-gcc
LD              = riscv64-unknown-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-unknown-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

3. Build CoreMark:

```bash
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ make PORT_DIR=linux64 link
riscv64-unknown-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:

```bash
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ ls -al
total 160
drwxr-xr-x 8 user user  4096 Mar 16 17:46 .
drwxr-xr-x 9 user user  4096 Mar 16 17:42 ..
-rw-r--r-- 1 user user  9416 May 23  2018 LICENSE.md
-rw-r--r-- 1 user user  3678 May 23  2018 Makefile
-rw-r--r-- 1 user user 18799 May 23  2018 README.md
drwxr-xr-x 2 user user  4096 May 23  2018 barebones
-rw-r--r-- 1 user user 14651 May 23  2018 core_list_join.c
-rw-r--r-- 1 user user 12503 May 23  2018 core_main.c
-rw-r--r-- 1 user user  8097 May 23  2018 core_matrix.c
-rw-r--r-- 1 user user  7186 May 23  2018 core_state.c
-rw-r--r-- 1 user user  5171 May 23  2018 core_util.c
-rwxr-xr-x 1 user user 27680 Mar 16 17:46 coremark.exe
-rw-r--r-- 1 user user  4373 May 23  2018 coremark.h
drwxr-xr-x 2 user user  4096 May 23  2018 cygwin
drwxr-xr-x 3 user user  4096 May 23  2018 docs
drwxr-xr-x 2 user user  4096 May 23  2018 linux
drwxr-xr-x 2 user user  4096 Mar 16 17:44 linux64
drwxr-xr-x 2 user user  4096 May 23  2018 simple
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$

«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=42284b8365034fa7c6428be87b1cd8ac4ea64697, for GNU/Linux 4.15.0, with debug_info, not stripped
```

5. CoreMark score

```bash
«Ruyi venv-gnu-upstream» user@milkv:~/coremark_upstream$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 20758
Total time (secs): 20.758000
Iterations/Sec   : 5299.161769
Iterations       : 110000
Compiler version : GCC13.2.0
Compiler flags   : -O2   -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5299.161769 / GCC13.2.0 -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
| --------------------- | ----------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 5299.161769 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 20758       | Total number of clock cycles                                 |
| **Total time (secs)** | 20.758      | Total execution time in seconds                              |
| **Iterations**        | 110000      | Total number of iterations performed                         |
| **Compiler version**  | GCC13.2.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the  StarFive JH7110 (U74) SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Upstream Toolchain on StarFive JH7110 (U74):

| Test Case                  | Expected Result                        | Actual Result                                                                     | Status |
| -------------------------- | -------------------------------------- | --------------------------------------------------------------------------------- | ------ |
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `/root/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0` | ✅ PASS |
| **Compiler Verification**  | GCC 13.2.0 for RISC-V architecture     | GCC 13.2.0 with rv64gc architecture, lp64d ABI                                    | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                                                | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5299.161769           | ✅ PASS |

All tests passed successfully, confirming that the GNU Upstream Toolchain works correctly on the StarFive JH7110 (U74) SoC.
