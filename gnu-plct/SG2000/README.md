# Sophgo SG2000 GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information
- RuyiSDK on MilkV DuoS with Sophgo SG2000 SoC
- Testing date: March 15, 2025

### Hardware Information
- MilkV DuoS
- Sophgo SG2000 SoC

## Installation

### 1. Install Toolchain

**Command:**
```bash
ruyi install toolchain/gnu-plct
```

**Result:**
```bash
debian@duos:~$ ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Saturday
info: usage information has already been uploaded today at 2025-03-15 02:10:49 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /home/debian/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
  ---------------------------------------- 242.6/242.6 MB 2.4 MB/s 01:41
info: extracting RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20240324.0
info: package gnu-plct-0.20240324.0 installed to /home/debian/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0
```

### 2. Create Virtual Environment

**Command:**
```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result:**
```bash
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Saturday
info: usage information has already been uploaded today at 2025-03-15 02:10:49 +0000
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
```bash
debian@duos:~$ ls ~/venv-gnu-plct/
bin                                     sysroot
meson-cross.ini                         sysroot.riscv64-plct-linux-gnu
meson-cross.riscv64-plct-linux-gnu.ini  toolchain.cmake
ruyi-cache.v2.toml                      toolchain.riscv64-plct-linux-gnu.cmake
ruyi-venv.toml
debian@duos:~$ ls ~/venv-gnu-plct/bin
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

This step created a virtual environment at `~/venv-gnu-plct/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

### 3. Activate Environment

**Command:**
```bash
. ~/venv-gnu-plct/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:
```bash
«Ruyi venv-gnu-plct» debian@duos:~$
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
«Ruyi venv-gnu-plct» debian@duos:~$ riscv64-plct-linux-gnu-gcc -v
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
«Ruyi venv-gnu-plct» debian@duos:~$ cat hello.c
#include <stdio.h>

int main()
{
    printf("Hello World\n");
    return 0;
}
«Ruyi venv-gnu-plct» debian@duos:~$ riscv64-plct-linux-gnu-gcc hello.c -o hello && ./hello
Hello World
```

The program executed successfully and output "Hello World", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

#### Native Compile

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:

```bash
«Ruyi venv-gnu-plct» debian@duos:~$ mkdir coremark && cd coremark
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Saturday
info: usage information has already been uploaded today at 2025-03-15 02:10:49 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ ls
LICENSE.md  README.md  core_list_join.c  core_matrix.c  core_util.c  cygwin  linux    simple
Makefile    barebones  core_main.c       core_state.c   coremark.h   docs    linux64
```

2. Modify the build configuration to use the RISC-V compiler:

```bash
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ cat linux64/core_portme.mak | grep gcc
CC = gcc
LD              = gcc
# E.g. generate profile guidance files. Sample PGO generation for gcc enabled with PGO=1
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ cat linux64/core_portme.mak | grep gcc
CC = riscv64-plct-linux-gnu-gcc
LD              = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
```

3. Build CoreMark:

```bash
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:

```bash
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=2ec45ce18cb640e991e7f27da56169eaa1652368, for GNU/Linux 4.15.0, with debug_info, not stripped
```

#### Cross Compile

**System and Hardware Information:**
- OS: Arch Linux x86_64
- Kernel: 6.13.6-zen1-1-zen

**Commands and Results:**

- Install toolchain
```bash
[test@arch ruyisdk]$ ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-03-19 08:00:00 +0800 and 2025-03-20 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz to /home/test/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  238M  100  238M    0     0  14.7M      0  0:00:16  0:00:16 --:--:-- 16.7M
info: extracting RuyiSDK-20240324-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20240324.0
info: package gnu-plct-0.20240324.0 installed to /home/test/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20240324.0
```
- Create virtual environment
```bash
[test@arch ruyisdk]$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-03-19 08:00:00 +0800 and 2025-03-20 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-plct...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/test/Documents/ruyisdk/venv-gnu-plct/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

- Activate environment
```bash
[test@arch ruyisdk]$ ./venv-gnu-plct/bin/ruyi-activate
«Ruyi venv-gnu-plct» [test@arch ruyisdk]$ riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/test/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20240324.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/test/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20240324.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/13.1.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=x86_64-build_pc-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20240324 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --enable-libquadmath --enable-libquadmath-support --disable-libmpx --with-gmp=/work/riscv64-plct-linux-gnu/buildtools --with-mpfr=/work/riscv64-plct-linux-gnu/buildtools --with-mpc=/work/riscv64-plct-linux-gnu/buildtools --with-isl=/work/riscv64-plct-linux-gnu/buildtools --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 13.1.0 (RuyiSDK 20240324 PLCT-Sources)
```

- Extract and build CoreMark with Ruyi
```bash
«Ruyi venv-gnu-plct» [test@arch ruyisdk]$ mkdir coremark && cd coremark
«Ruyi venv-gnu-plct» [test@arch coremark]$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-03-19 08:00:00 +0800 and 2025-03-20 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/test/.cache/ruyi/distfiles/coremark-1.01.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  391k  100  391k    0     0   765k      0 --:--:-- --:--:-- --:--:--  766k
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-plct» [test@arch coremark]$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-plct» [test@arch coremark]$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
«Ruyi venv-gnu-plct» [test@arch coremark]$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=310ecde40710e1cccfffdf1f541cb610aa593b26, for GNU/Linux 4.15.0, with debug_info, not stripped
```

#### CoreMark score

```bash
«Ruyi venv-gnu-plct» debian@duos:~$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 11972
Total time (secs): 11.972000
Iterations/Sec   : 2505.846976
Iterations       : 30000
Compiler version : GCC13.1.0
Compiler flags   : -O2   -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x5275
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 2505.846976 / GCC13.1.0 -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
|-----------------------|-------------|--------------------------------------------------------------|
| **Iterations/Sec**    | 2505.846976 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 11972       | Total number of clock cycles                                 |
| **Total time (secs)** | 11.972000   | Total execution time in seconds                              |
| **Iterations**        | 30000       | Total number of iterations performed                         |
| **Compiler version**  | GCC13.1.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the Sophgo SG2000 SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Sophgo SG2000 SoC:

| Test Case                  | Expected Result                              | Actual Result                                                             | Status  |
|----------------------------|----------------------------------------------|---------------------------------------------------------------------------|---------|
| **Toolchain Installation** | Successfully installed toolchain             | Installed to `~/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0` | ✅ PASS |
| **Compiler Verification**  | GCC 13.1.0 for RISC-V architecture           | GCC 13.1.0 with rv64gc architecture, lp64d ABI                            | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution         | Successfully compiled and executed                                        | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark       | Successfully compiled and completed benchmark with score of 2505.846976   | ✅ PASS |

All tests passed successfully, confirming that the GNU Toolchain works correctly on the Sophgo SG2000 SoC.
