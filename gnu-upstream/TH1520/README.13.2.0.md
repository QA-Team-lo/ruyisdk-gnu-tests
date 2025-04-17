# LicheePi 4A GNU Toolchain (gnu-upstream) Test Report

## Environment

### System Information

- RuyiSDK on LicheePi 4A (8G RAM, 32G eMMC) with TH1520 SoC
- Testing date: March 20, 2025

### Hardware Information
- LicheePi 4A (8G RAM, 32G eMMC)
- TH1520 SoC (4 * XuanTie C910 Core @ 2GHz)
- OS: RevyOS 20250123

## Installation

### 0. Install ruyi package manager

```bash
sudo apt update && sudo apt upgrade
sudo apt install wget
wget https://mirror.iscas.ac.cn/ruyisdk/ruyi/releases/0.29.0/ruyi.riscv64
sudo mv ruyi.riscv64 /usr/bin/ruyi
sudo chmod +x /usr/bin/ruyi
```

### 1. Install Toolchain

`ruyi` is dependent on xz-utils, if not installed, please install it first.

```bash
sudo apt update && sudo apt upgrade
sudo apt install xz-utils
```

**Command:**

```bash
ruyi install toolchain/gnu-upstream
```

**Result:**

```log
debian@revyos-lpi4a:~$ ruyi install toolchain/gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: the next upload will happen anytime ruyi is executed between 2025-03-21 08:00:00 +0800 and 2025-03-22 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz to /home/debian/.cache/ruyi/distfiles/RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  224M  100  224M    0     0  2291k      0  0:01:40  0:01:40 --:--:-- 2201k
info: extracting RuyiSDK-20231212-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz for package gnu-upstream-0.20231212.0
info: package gnu-upstream-0.20231212.0 installed to /home/debian/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0
```

This command downloaded the toolchain package (~224MB) from ISCAS mirror and installed it to `~/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0`.

### 2. Create Virtual Environment

```bash
ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
```

**Result:** 

```log
debian@revyos-lpi4a:~$ ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: the next upload will happen anytime ruyi is executed between 2025-03-21 08:00:00 +0800 and 2025-03-22 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-upstream...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/debian/venv-gnu-upstream/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**

```log
debian@revyos-lpi4a:~$ ls venv-gnu-upstream/
bin                                        sysroot
meson-cross.ini                            sysroot.riscv64-unknown-linux-gnu
meson-cross.riscv64-unknown-linux-gnu.ini  toolchain.cmake
ruyi-cache.v2.toml                         toolchain.riscv64-unknown-linux-gnu.cmake
ruyi-venv.toml
debian@revyos-lpi4a:~$ ls venv-gnu-upstream/bin
riscv64-unknown-linux-gnu-addr2line   riscv64-unknown-linux-gnu-gdb-add-index
riscv64-unknown-linux-gnu-ar          riscv64-unknown-linux-gnu-gfortran
riscv64-unknown-linux-gnu-as          riscv64-unknown-linux-gnu-gprof
riscv64-unknown-linux-gnu-c++         riscv64-unknown-linux-gnu-ld
riscv64-unknown-linux-gnu-cc          riscv64-unknown-linux-gnu-ld.bfd
riscv64-unknown-linux-gnu-c++filt     riscv64-unknown-linux-gnu-ldd
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

This step created a virtual environment at `~/venv-gnu-upstream/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-unknown-linux-gnu-`.

### 3. Activate Environment

**Command:**

```bash
source ~/venv-gnu-upstream/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:

```log
debian@revyos-lpi4a:~$ source ~/venv-gnu-upstream/bin/ruyi-activate
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~$ 
```

This initialized the environment and provided access to all cross-compilation tools.

## Tests & Results

### 1. Compiler Version Check

**Command:**

```bash
riscv64-unknown-linux-gnu-gcc -v
```

**Result:**

```log
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~$ riscv64-unknown-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0/bin/riscv64-unknown-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0/bin/../libexec/gcc/riscv64-unknown-linux-gnu/13.2.0/lto-wrapper
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
echo '
#include <stdio.h>

int main() {
    printf("Hello, World! From LPi4A with gnu-upstream\n");
    return 0;
}
' > hello.c
riscv64-unknown-linux-gnu-gcc hello.c -o hello_upstream
./hello_upstream 
```

**Result:**

```log
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~$ echo '
#include <stdio.h>

int main() {
    printf("Hello, World! From LPi4A with gnu-upstream\n");
    return 0;
}
' > hello.c
riscv64-unknown-linux-gnu-gcc hello.c -o hello_upstream
./hello_upstream 
Hello, World! From LPi4A with gnu-upstream
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~$ 
```

The program executed successfully and output "Hello, World! From LPi4A with gnu-upstream", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:

```bash
mkdir coremark && cd coremark
ruyi extract coremark
```

```log
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~$ mkdir coremark && cd coremark
ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: the next upload will happen anytime ruyi is executed between 2025-03-21 08:00:00 +0800 and 2025-03-22 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/debian/.cache/ruyi/distfiles/coremark-1.01.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--    10  391k   10 43440    0     0  62749      0  0:00:06 --:--:--  0:00:06 62100  391k  100  391k    0     0   341k      0  0:00:01  0:00:01 --:--:--  341k
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~/coremark$ 
```

2. Modify the build configuration to use the RISC-V compiler:

```bash
sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
```

```log
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~/coremark$ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~/coremark$ cat ./linux64/core_portme.mak | grep "gcc"
CC = riscv64-unknown-linux-gnu-gcc
LD              = riscv64-unknown-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-unknown-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~/coremark$ 
```

3. Build CoreMark:

*If you don't have `make` installed, please install it first: `sudo apt install make`*

```bash
make PORT_DIR=linux64 link
```

```log
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~/coremark$ make PORT_DIR=linux64 link
riscv64-unknown-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:

```bash
file coremark.exe
```

```log
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~/coremark$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=42284b8365034fa7c6428be87b1cd8ac4ea64697, for GNU/Linux 4.15.0, with debug_info, not stripped
```

5. CoreMark score:

```bash
./coremark.exe
```

```log
«Ruyi venv-gnu-upstream» debian@revyos-lpi4a:~/coremark$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 13476
Total time (secs): 13.476000
Iterations/Sec   : 8162.659543
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
CoreMark 1.0 : 8162.659543 / GCC13.2.0 -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
| --------------------- | ----------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 8162.659543 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 13476       | Total number of clock cycles                                 |
| **Total time (secs)** | 13.476000   | Total execution time in seconds                              |
| **Iterations**        | 110000      | Total number of iterations performed                         |
| **Compiler version**  | GCC13.2.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |


These results demonstrate good performance of the TH1520 SoC on Lichee Pi 4A when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Upstream Toolchain on LicheePi 4A (TH1520 SoC):

| Test Case                  | Expected Result                        | Actual Result                                                                     | Status |
| -------------------------- | -------------------------------------- | --------------------------------------------------------------------------------- | ------ |
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `~/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20231212.0` | ✅ PASS |
| **Compiler Verification**  | GCC 13.2.0 for RISC-V architecture     | GCC 13.2.0 with rv64gc architecture, lp64d ABI                                    | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                                                | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 8162.659543           | ✅ PASS |

All tests passed successfully, confirming that the PLCT Upstream Toolchain works correctly on the TH1520 SoC.
