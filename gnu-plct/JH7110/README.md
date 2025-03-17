# StarFive JH7110 (U74) GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information

- RuyiSDK on Milk-V Mars with JH7110 (U74) SoC
- OS: Milk-V Mars official Debian image (Releases V1.0.6)
- Testing date: March 16, 2025

### Hardware Information

- Milk-V Mars board (8GB RAM) with Ethernet connection and 64GB SD card
- StarFive JH7110 SoC (SiFive U74 core)

## Installation

### 0. Install Ruyi

```bash
sudo apt update
sudo apt install wget xz-utils make
sudo wget https://mirror.iscas.ac.cn/ruyisdk/ruyi/releases/0.28.0/ruyi.riscv64
sudo cp -v ruyi.riscv64 /usr/local/bin/ruyi  # Copy to your $PATH
sudo chmod +x /usr/local/bin/ruyi
```

### 1. Install Toolchain

**Command:**

```bash
ruyi install toolchain/gnu-plct
```

**Result:**

```bash
user@milkv:~$ ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-03-20 00:00:00 +0000 and 2025-03-21 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /home/user/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
--2025-03-16 14:54:45--  https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 242595644 (231M) [application/octet-stream]
Saving to: ‘/home/user/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz’

/home/user/.cache/r 100%[===================>] 231.36M  8.42MB/s    in 25s

2025-03-16 14:55:11 (9.12 MB/s) - ‘/home/user/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz’ saved [242595644/242595644]

info: extracting RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20240324.0

info: package gnu-plct-0.20240324.0 installed to /home/user/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0
```

This command downloaded the toolchain package (~231MB) from ISCAS mirror and installed it to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0`.

### 2. Create Virtual Environment

**Command:**

```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result:**

```bash
user@milkv:~$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-03-20 00:00:00 +0000 and 2025-03-21 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-plct...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/user/venv-gnu-plct/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**

```bash
user@milkv:~$ ls ~/venv-gnu-plct/
bin                                     sysroot
meson-cross.ini                         sysroot.riscv64-plct-linux-gnu
meson-cross.riscv64-plct-linux-gnu.ini  toolchain.cmake
ruyi-cache.v2.toml                      toolchain.riscv64-plct-linux-gnu.cmake
ruyi-venv.toml

user@milkv:~$ ls ~/venv-gnu-plct/bin/
riscv64-plct-linux-gnu-addr2line   riscv64-plct-linux-gnu-gdb-add-index
riscv64-plct-linux-gnu-ar          riscv64-plct-linux-gnu-gfortran
riscv64-plct-linux-gnu-as          riscv64-plct-linux-gnu-gprof
riscv64-plct-linux-gnu-c++         riscv64-plct-linux-gnu-ld
riscv64-plct-linux-gnu-c++filt     riscv64-plct-linux-gnu-ld.bfd
riscv64-plct-linux-gnu-cc          riscv64-plct-linux-gnu-ldd
riscv64-plct-linux-gnu-cpp         riscv64-plct-linux-gnu-lto-dump
riscv64-plct-linux-gnu-elfedit     riscv64-plct-linux-gnu-nm
riscv64-plct-linux-gnu-g++         riscv64-plct-linux-gnu-objcopy
riscv64-plct-linux-gnu-gcc         riscv64-plct-linux-gnu-objdump
riscv64-plct-linux-gnu-gcc-ar      riscv64-plct-linux-gnu-ranlib
riscv64-plct-linux-gnu-gcc-nm      riscv64-plct-linux-gnu-readelf
riscv64-plct-linux-gnu-gcc-ranlib  riscv64-plct-linux-gnu-size
riscv64-plct-linux-gnu-gcov        riscv64-plct-linux-gnu-strings
riscv64-plct-linux-gnu-gcov-dump   riscv64-plct-linux-gnu-strip
riscv64-plct-linux-gnu-gcov-tool   ruyi-activate
riscv64-plct-linux-gnu-gdb
```

This step created a virtual environment at `/home/'your user'/venv-gnu-plct/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

### 3. Activate Environment

**Command:**

```bash
. ~/venv-gnu-plct/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:

```bash
user@milkv:~$ . ~/venv-gnu-plct/bin/ruyi-activate
«Ruyi venv-gnu-plct» user@milkv:~$
```

This initialized the environment and provided access to all cross-compilation tools.

## Tests & Results

### 1. Compiler Version Check

**Command:**

```bash
riscv64-plct-linux-gnu-gcc -v
```

**Result:**

```bash
«Ruyi venv-gnu-plct» user@milkv:~$ riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/user/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/user/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/13.1.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20240324 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --enable-libquadmath --enable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 13.1.0 (RuyiSDK 20240324 PLCT-Sources)
```

The output confirmed a successful installation showing:

- GCC version: 13.1.0 (RuyiSDK 20240324 PLCT-Sources)
- Target architecture: riscv64-plct-linux-gnu
- Thread model: posix
- Configured with appropriate RISC-V specific options (rv64gc architecture, lp64d ABI)

### 2. Hello World Program

**Program**

```bash
«Ruyi venv-gnu-plct» user@milkv:~$ cat Hello_Mars.c
#include <stdio.h>

int main(void)
{
    printf("Milk-V Mars: Hello World!\n");
    return 0;
}
«Ruyi venv-gnu-plct» user@milkv:~$
```

**Commands:**

```bash
«Ruyi venv-gnu-plct» user@milkv:~$ riscv64-plct-linux-gnu-gcc Hello_Mars.c -o Hello_Mars
«Ruyi venv-gnu-plct» user@milkv:~$ ./Hello_Mars
Milk-V Mars: Hello World!
«Ruyi venv-gnu-plct» user@milkv:~$
```

The program executed successfully and output "Milk-V Mars: Hello World!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark (-O2 -lrt)

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:

```bash
«Ruyi venv-gnu-plct» user@milkv:~$ mkdir coremark && cd coremark/
«Ruyi venv-gnu-plct» user@milkv:~/coremark$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-03-20 00:00:00 +0000 and 2025-03-21 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/user/.cache/ruyi/distfiles/coremark-1.01.tar.gz
--2025-03-16 15:32:54--  https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 401208 (392K) [application/octet-stream]
Saving to: ‘/home/user/.cache/ruyi/distfiles/coremark-1.01.tar.gz’

/home/user/.cache/r 100%[===================>] 391.80K  1.23MB/s    in 0.3s

2025-03-16 15:32:55 (1.23 MB/s) - ‘/home/user/.cache/ruyi/distfiles/coremark-1.01.tar.gz’ saved [401208/401208]

info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

2. Modify the build configuration to use the RISC-V compiler:

```bash
«Ruyi venv-gnu-plct» user@milkv:~/coremark# sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' ./linux64/core_portme.mak

«Ruyi venv-gnu-plct» user@milkv:~/coremark$ cat ./linux64/core_portme.mak | grep "gcc"
CC = riscv64-plct-linux-gnu-gcc
LD              = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

3. Build CoreMark:

```bash
«Ruyi venv-gnu-plct» user@milkv:~/coremark$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:

```bash
«Ruyi venv-gnu-plct» user@milkv:~/coremark$ ls -al
total 160
drwxr-xr-x 8 user user  4096 Mar 16 15:48 .
drwxr-xr-x 7 user user  4096 Mar 16 15:46 ..
-rw-r--r-- 1 user user  9416 May 23  2018 LICENSE.md
-rw-r--r-- 1 user user  3678 May 23  2018 Makefile
-rw-r--r-- 1 user user 18799 May 23  2018 README.md
drwxr-xr-x 2 user user  4096 May 23  2018 barebones
-rw-r--r-- 1 user user 14651 May 23  2018 core_list_join.c
-rw-r--r-- 1 user user 12503 May 23  2018 core_main.c
-rw-r--r-- 1 user user  8097 May 23  2018 core_matrix.c
-rw-r--r-- 1 user user  7186 May 23  2018 core_state.c
-rw-r--r-- 1 user user  5171 May 23  2018 core_util.c
-rwxr-xr-x 1 user user 28152 Mar 16 15:48 coremark.exe
-rw-r--r-- 1 user user  4373 May 23  2018 coremark.h
drwxr-xr-x 2 user user  4096 May 23  2018 cygwin
drwxr-xr-x 3 user user  4096 May 23  2018 docs
drwxr-xr-x 2 user user  4096 May 23  2018 linux
drwxr-xr-x 2 user user  4096 Mar 16 15:43 linux64
drwxr-xr-x 2 user user  4096 May 23  2018 simple

«Ruyi venv-gnu-plct» user@milkv:~/coremark$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=2ec45ce18cb640e991e7f27da56169eaa1652368, for GNU/Linux 4.15.0, with debug_info, not stripped
```

5. CoreMark score

```bash
«Ruyi venv-gnu-plct» user@milkv:~/coremark$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 20604
Total time (secs): 20.604000
Iterations/Sec   : 5338.769171
Iterations       : 110000
Compiler version : GCC13.1.0
Compiler flags   : -O2   -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5338.769171 / GCC13.1.0 -O2   -lrt / Heap
```

### 4. CoreMark Results Comparison

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results with different compilation flags are summarized in the table below:

| Metric                | Value | Description |
|--------               |---------------|-------------|
| **Iterations/Sec**    | 5338.769171   | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 20604         | Total number of clock cycles |
| **Total time (secs)** | 20.604        |  Total execution time in seconds |
| **Iterations**        | 110000        | Total number of iterations performed |
| **Compiler version**  | GCC13.1.0     | Compiler used for the test |
| **Compiler flags**    | -O2 -lrt      | Compilation flags used |
| **Memory location**   | Heap          | Where data is stored during execution |

These results demonstrate the performance of the StarFive JH7110 (U74) SoC when running CoreMark with different compilation flags.

## Test Summary

The following table summarizes the test results for GNU Toolchain on StarFive JH7110 (U74) SoC:

| Test Case | Expected Result | Actual Result | Status |
|-----------|----------------|---------------|--------|
| **Toolchain Installation** | Successfully installed toolchain | Installed to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0` | ✅ PASS |
| **Compiler Verification** | GCC 13.1.0 for RISC-V architecture | GCC 13.1.0 with rv64gc architecture, lp64d ABI | ✅ PASS |
| **Hello World Test** | Successful compilation and execution | Successfully compiled and executed | ✅ PASS |
| **CoreMark Benchmark (Default)** | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5338.769171 | ✅ PASS |

All tests passed successfully, confirming that the PLCT GNU Toolchain works correctly on the StarFive JH7110 (U74) SoC.
