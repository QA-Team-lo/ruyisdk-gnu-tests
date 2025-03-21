# LicheePi 4A GNU Toolchain (gnu-plct) Test Report

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
ruyi install toolchain/gnu-plct
```

**Result:**

```log
debian@revyos-lpi4a:~$ ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: the next upload will happen anytime ruyi is executed between 2025-03-21 08:00:00 +0800 and 2025-03-22 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /home/debian/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  231M  100  231M    0     0  3422k      0  0:01:09  0:01:09 --:--:-- 4000k  231M    0  668k    0     0   774k      0  0:05:06 --:--:--  0:05:06  7  1  231M    1 3964k    0     0  2129k      0  0:01:51  0:0
info: extracting RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20240324.0
info: package gnu-plct-0.20240324.0 installed to /home/debian/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0

```

This command downloaded the toolchain package (~224MB) from ISCAS mirror and installed it to `~/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0`.

### 2. Create Virtual Environment

```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result:** 

```log
debian@revyos-lpi4a:~$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: the next upload will happen anytime ruyi is executed between 2025-03-21 08:00:00 +0800 and 2025-03-22 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-plct...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/debian/venv-gnu-plct/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.

```

**Verification of created environment contents:**

```log
debian@revyos-lpi4a:~$ ls ~/venv-gnu-plct/
bin
meson-cross.ini
meson-cross.riscv64-plct-linux-gnu.ini
ruyi-cache.v2.toml
ruyi-venv.toml
sysroot
sysroot.riscv64-plct-linux-gnu
toolchain.cmake
toolchain.riscv64-plct-linux-gnu.cmake
debian@revyos-lpi4a:~$ ls ~/venv-gnu-plct/bin/
riscv64-plct-linux-gnu-addr2line   riscv64-plct-linux-gnu-gdb-add-index
riscv64-plct-linux-gnu-ar          riscv64-plct-linux-gnu-gfortran
riscv64-plct-linux-gnu-as          riscv64-plct-linux-gnu-gprof
riscv64-plct-linux-gnu-c++         riscv64-plct-linux-gnu-ld
riscv64-plct-linux-gnu-cc          riscv64-plct-linux-gnu-ld.bfd
riscv64-plct-linux-gnu-c++filt     riscv64-plct-linux-gnu-ldd
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

This step created a virtual environment at `~/venv-gnu-plct/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

### 3. Activate Environment

**Command:**

```bash
source ~/venv-gnu-plct/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:

```log
debian@revyos-lpi4a:~$ source ~/venv-gnu-plct/bin/ruyi-activate
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~$ 

```

This initialized the environment and provided access to all cross-compilation tools.

## Tests & Results

### 1. Compiler Version Check

**Command:**

```bash
riscv64-plct-linux-gnu-gcc -v
```

**Result:**

```log
debian@revyos-lpi4a:~$ source ~/venv-gnu-plct/bin/ruyi-activate
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~$ riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/13.1.0/lto-wrapper
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

**Commands:**

```bash
echo '
#include <stdio.h>

int main() {
    printf("Hello, World! From LPi4A with gnu-plct\n");
    return 0;
}
' > hello.c
riscv64-plct-linux-gnu-gcc hello.c -march=rv64gc_xtheadvector -o hello_gnuplct
./hello_gnuplct 
```

**Result:**

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~$ echo '
#include <stdio.h>

int main() {
    printf("Hello, World! From LPi4A with gnu-plct\n");
    return 0;
}
' > hello.c
riscv64-plct-linux-gnu-gcc hello.c -march=rv64gc_xtheadvector -o hello_gnuplct
./hello_gnuplct 
Hello, World! From LPi4A with gnu-plct

```

The program executed successfully and output "Hello, World! From LPi4A with gnu-plct", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:

```bash
mkdir coremark && cd coremark
ruyi extract coremark
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~$ mkdir coremark && cd coremark
ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: the next upload will happen anytime ruyi is executed between 2025-03-21 08:00:00 +0800 and 2025-03-22 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ 
```

2. Modify the build configuration to use the RISC-V compiler:

```bash
sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ cd ..
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~$ rm -rf coremark/«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~$ mkdir coremark && cd coremark«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ ruyi extract coremarkwarn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: the next upload will happen anytime ruyi is executed between 2025-03-21 08:00:00 +0800 and 2025-03-22 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ cat ./linux64/core_portme.mak | grep "gcc"
CC = riscv64-plct-linux-gnu-gcc
LD              = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:

```

#### 3.1 CoreMark Benchmark (march: rv64gc)

1. Build CoreMark:

*If you don't have `make` installed, please install it first: `sudo apt install make`*

```bash
make PORT_DIR=linux64 link
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile

```

2. Verify the resulting binary is a RISC-V executable:

```bash
file coremark.exe
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ file coremark.exe 
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=2ec45ce18cb640e991e7f27da56169eaa1652368, for GNU/Linux 4.15.0, with debug_info, not stripped

```

3. CoreMark score:

```bash
./coremark.exe
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ ./coremark.exe 2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 13254
Total time (secs): 13.254000
Iterations/Sec   : 8299.381319
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
CoreMark 1.0 : 8299.381319 / GCC13.1.0 -O2   -lrt / Heap

```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
| --------------------- | ----------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 8299.381319 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 13254       | Total number of clock cycles                                 |
| **Total time (secs)** | 13.254000   | Total execution time in seconds                              |
| **Iterations**        | 110000      | Total number of iterations performed                         |
| **Compiler version**  | GCC13.1.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |


These results demonstrate good performance of the TH1520 SoC on Lichee Pi 4A when running CoreMark compiled with the RuyiSDK toolchain and march rv64gc.

#### 3.2 CoreMark Benchmark (march: rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs)

1. Build CoreMark:

*If you don't have `make` installed, please install it first: `sudo apt install make`*

```bash
make PORT_DIR=linux64 link XCFLAGS="-march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs"
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ make PORT_DIR=linux64 link XCFLAGS="-march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs"
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs  -lrt"\" -DITERATIONS=0 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile

```

2. Verify the resulting binary is a RISC-V executable:

```bash
file coremark.exe
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=54a2578c49e88f003ddd397de5ecec1a19379d82, for GNU/Linux 4.15.0, with debug_info, not stripped

```

3. CoreMark score:

```bash
./coremark.exe
```

```log
«Ruyi venv-gnu-plct» debian@revyos-lpi4a:~/coremark$ ./coremark.exe 
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 13182
Total time (secs): 13.182000
Iterations/Sec   : 8344.712487
Iterations       : 110000
Compiler version : GCC13.1.0
Compiler flags   : -O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs  -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 8344.712487 / GCC13.1.0 -O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs  -lrt / Heap

```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value                                                           | Description                                                  |
| --------------------- | --------------------------------------------------------------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 8344.712487                                                     | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 13182                                                           | Total number of clock cycles                                 |
| **Total time (secs)** | 13.182000                                                       | Total execution time in seconds                              |
| **Iterations**        | 110000                                                          | Total number of iterations performed                         |
| **Compiler version**  | GCC13.1.0                                                       | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -march=rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs  -lrt | Compilation flags used                                       |
| **Memory location**   | Heap                                                            | Where data is stored during execution                        |


These results demonstrate good performance of the TH1520 SoC on Lichee Pi 4A when running CoreMark compiled with the RuyiSDK toolchain and march rv64gc.

## Test Summary

The following table summarizes the test results for GNU Upstream Toolchain on LicheePi 4A (TH1520 SoC):

| Test Case                                                               | Expected Result                        | Actual Result                                                             | Status |
| ----------------------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------------- | ------ |
| **Toolchain Installation**                                              | Successfully installed toolchain       | Installed to `~/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0` | ✅ PASS |
| **Compiler Verification**                                               | GCC 13.1.0 for RISC-V architecture     | GCC 13.2.0 with rv64gc architecture, lp64d ABI                            | ✅ PASS |
| **Hello World Test**                                                    | Successful compilation and execution   | Successfully compiled and executed                                        | ✅ PASS |
| **CoreMark Benchmark (rv64gc)**                                         | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 8299.381319   | ✅ PASS |
| **CoreMark Benchmark (rv64gc_xtheadvector_xtheadba_xtheadbb_xtheadbs)** | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 8344.712487   | ✅ PASS |

All tests passed successfully, confirming that the PLCT GNU Toolchain works correctly on the TH1520 SoC.

