# Sophgo SG2000 GNU Toolchain (gnu-upstream) Test Report

## Environment

### System Information
- RuyiSDK on MilkV DuoS with Sophgo SG2000 SoC
- Testing date: April 15, 2025

### Hardware Information
- MilkV DuoS
- Sophgo SG2000 SoC

## Installation

### 1. Install Toolchain

**Command:**
```bash
ruyi install toolchain/gnu-upstream
```

**Result:**
```bash
debian@duos:~$ ruyi install toolchain/gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-04-16 00:00:00 +0000 and 2025-04-17 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz to /home/debian/.cache/ruyi/distfiles/RuyiSDK-20250401-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz
--2025-04-15 13:28:43--  https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 168839664 (161M) [application/octet-stream]
Saving to: '/home/debian/.cache/ruyi/distfiles/RuyiSDK-20250401-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz'

/home/debian/.cache/ruyi/distfiles/RuyiSDK-20250401- 100%[====================================================================================================================>] 161.02M  3.39MB/s    in 49s

2025-04-15 13:29:33 (3.27 MB/s) - '/home/debian/.cache/ruyi/distfiles/RuyiSDK-20250401-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz' saved [168839664/168839664]

info: extracting RuyiSDK-20250401-Upstream-Sources-HOST-riscv64-linux-gnu-riscv64-unknown-linux-gnu.tar.xz for package gnu-upstream-0.20250401.0
info: package gnu-upstream-0.20250401.0 installed to /home/debian/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20250401.0
```

### 2. Create Virtual Environment

**Command:**
```bash
ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
```

**Result:**
```bash
debian@duos:~$ ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-04-16 00:00:00 +0000 and 2025-04-17 00:00:00 +0000
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
```bas
debian@duos:~$ ls ~/venv-gnu-upstream/
bin  meson-cross.ini  meson-cross.riscv64-unknown-linux-gnu.ini  ruyi-cache.v2.toml  ruyi-venv.toml  sysroot  sysroot.riscv64-unknown-linux-gnu  toolchain.cmake  toolchain.riscv64-unknown-linux-gnu.cmake
debian@duos:~$ ls ~/venv-gnu-upstream/bin
riscv64-unknown-linux-gnu-addr2line  riscv64-unknown-linux-gnu-g++         riscv64-unknown-linux-gnu-gdb              riscv64-unknown-linux-gnu-gprof     riscv64-unknown-linux-gnu-objcopy
riscv64-unknown-linux-gnu-ar         riscv64-unknown-linux-gnu-gcc         riscv64-unknown-linux-gnu-gdb-add-index    riscv64-unknown-linux-gnu-gprofng   riscv64-unknown-linux-gnu-objdump
riscv64-unknown-linux-gnu-as         riscv64-unknown-linux-gnu-gcc-ar      riscv64-unknown-linux-gnu-gfortran         riscv64-unknown-linux-gnu-gstack    riscv64-unknown-linux-gnu-ranlib
riscv64-unknown-linux-gnu-c++        riscv64-unknown-linux-gnu-gcc-nm      riscv64-unknown-linux-gnu-gp-archive       riscv64-unknown-linux-gnu-ld        riscv64-unknown-linux-gnu-readelf
riscv64-unknown-linux-gnu-c++filt    riscv64-unknown-linux-gnu-gcc-ranlib  riscv64-unknown-linux-gnu-gp-collect-app   riscv64-unknown-linux-gnu-ld.bfd    riscv64-unknown-linux-gnu-size
riscv64-unknown-linux-gnu-cc         riscv64-unknown-linux-gnu-gcov        riscv64-unknown-linux-gnu-gp-display-html  riscv64-unknown-linux-gnu-ldd       riscv64-unknown-linux-gnu-strings
riscv64-unknown-linux-gnu-cpp        riscv64-unknown-linux-gnu-gcov-dump   riscv64-unknown-linux-gnu-gp-display-src   riscv64-unknown-linux-gnu-lto-dump  riscv64-unknown-linux-gnu-strip
riscv64-unknown-linux-gnu-elfedit    riscv64-unknown-linux-gnu-gcov-tool   riscv64-unknown-linux-gnu-gp-display-text  riscv64-unknown-linux-gnu-nm        ruyi-activate
```

This step created a virtual environment at `~/venv-gnu-upstream/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-unknown-linux-gnu-`.

### 3. Activate Environment

**Command:**
```bash
. ~/venv-gnu-upstream/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:
```bash
«Ruyi venv-gnu-upstream» debian@duos:~$
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
debian@duos:~$ . ~/venv-gnu-upstream/bin/ruyi-activate
«Ruyi venv-gnu-upstream» debian@duos:~$ riscv64-unknown-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20250401.0/bin/riscv64-unknown-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20250401.0/bin/../libexec/gcc/riscv64-unknown-linux-gnu/14.2.0/lto-wrapper
Target: riscv64-unknown-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-unknown-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250401 Upstream-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --disable-multilib --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 14.2.0 (RuyiSDK 20250401 Upstream-Sources)
```

The output confirmed a successful installation showing:
- GCC version: 14.2.0 (RuyiSDK 20250401 Upstream-Sources)
- Target architecture: riscv64-unknown-linux-gnu
- Thread model: posix
- Configured with appropriate RISC-V specific options (rv64gc architecture, lp64d ABI)

### 2. Hello World Program

**Commands:**
```bash
«Ruyi venv-gnu-upstream» debian@duos:~$ cat hello.c
#include <stdio.h>

int main()
{
    printf("Hello World\n");
    return 0;
}
«Ruyi venv-gnu-upstream» debian@duos:~$ riscv64-unknown-linux-gnu-gcc hello.c -o hello && ./hello
Hello World
```

The program executed successfully and output "Hello World", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

#### Native Compile

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:

**Command:**

```bash
mkdir coremark && cd coremark
ruyi extract coremark
```

**Result:**

```bash
«Ruyi venv-gnu-upstream» debian@duos:~$ mkdir coremark && cd coremark
«Ruyi venv-gnu-upstream» debian@duos:~/coremark$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-04-16 00:00:00 +0000 and 2025-04-17 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-upstream» debian@duos:~/coremark$ ls
LICENSE.md  Makefile  README.md  barebones  core_list_join.c  core_main.c  core_matrix.c  core_state.c  core_util.c  coremark.h  cygwin  docs  linux  linux64  simple
```

2. Modify the build configuration to use the RISC-V compiler:

**Command:**
```bash
sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
```

**Result:**
```bash
«Ruyi venv-gnu-upstream» debian@duos:~/coremark$ cat linux64/core_portme.mak | grep gcc
CC = gcc
LD              = gcc
# E.g. generate profile guidance files. Sample PGO generation for gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
«Ruyi venv-gnu-upstream» debian@duos:~/coremark$ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-upstream» debian@duos:~/coremark$ cat linux64/core_portme.mak | grep gcc
CC = riscv64-unknown-linux-gnu-gcc
LD              = riscv64-unknown-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-unknown-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

3. Build CoreMark:

**Command:**
```bash
make PORT_DIR=linux64 link
```

**Result:**
```bash
«Ruyi venv-gnu-upstream» debian@duos:~/coremark$ make PORT_DIR=linux64 link
riscv64-unknown-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:

**Command:**
```bash
file coremark.exe
```

**Result:**
```bash
«Ruyi venv-gnu-upstream» debian@duos:~/coremark$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=5c8618cf62e0f1f7dd462ba5bddb03479631d0e9, for GNU/Linux 4.15.0, with debug_info, not stripped
```

#### Cross Compile

**System and Hardware Information:**
- OS: Arch Linux x86_64
- Kernel: 6.13.6-zen1-1-zen

**Commands and Results:**

1. Install toolchain

**Command:**
```bash
ruyi install toolchain/gnu-upstream
```

**Result:**
```bash
[test@arch ruyisdk]$ ruyi install toolchain/gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-04-16 08:00:00 +0800 and 2025-04-17 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz to /home/test/.cache/ruyi/distfiles/RuyiSDK-20250401-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  164M  100  164M    0     0  28.3M      0  0:00:05  0:00:05 --:--:-- 30.7M
info: extracting RuyiSDK-20250401-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz for package gnu-upstream-0.20250401.0
info: package gnu-upstream-0.20250401.0 installed to /home/test/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20250401.0
```
2. Create virtual environment

**Command:**
```bash
ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
```

**Result:**
```bash
[test@arch ruyisdk]$ ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-04-16 08:00:00 +0800 and 2025-04-17 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-upstream...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/test/Documents/ruyisdk/venv-gnu-upstream/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

3. Activate environment

**Command:**
```bash
. ./venv-gnu-upstream/bin/ruyi-activate
```

**Result:**
```bash
[test@arch ruyisdk]$ . ./venv-gnu-upstream/bin/ruyi-activate
«Ruyi venv-gnu-upstream» [test@arch ruyisdk]$ riscv64-unknown-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/test/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20250401.0/bin/riscv64-unknown-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/test/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20250401.0/bin/../libexec/gcc/riscv64-unknown-linux-gnu/14.2.0/lto-wrapper
Target: riscv64-unknown-linux-gnu
Configured with: /work/riscv64-unknown-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=x86_64-build_pc-linux-gnu --target=riscv64-unknown-linux-gnu --prefix=/opt/ruyi/riscv64-unknown-linux-gnu --exec_prefix=/opt/ruyi/riscv64-unknown-linux-gnu --with-sysroot=/opt/ruyi/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250401 Upstream-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/riscv64-unknown-linux-gnu/buildtools --with-mpfr=/work/riscv64-unknown-linux-gnu/buildtools --with-mpc=/work/riscv64-unknown-linux-gnu/buildtools --with-isl=/work/riscv64-unknown-linux-gnu/buildtools --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --disable-multilib --with-local-prefix=/opt/ruyi/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 14.2.0 (RuyiSDK 20250401 Upstream-Sources)
```

4. Extract and build CoreMark with Ruyi

**Command:**
```bash
ruyi extract coremark
sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
make PORT_DIR=linux64 link
```

**Result:**
```bash
«Ruyi venv-gnu-upstream» [test@arch ruyisdk]$ md coremark
«Ruyi venv-gnu-upstream» [test@arch coremark]$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-04-16 08:00:00 +0800 and 2025-04-17 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-upstream» [test@arch coremark]$ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-upstream» [test@arch coremark]$ make PORT_DIR=linux64 link
riscv64-unknown-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
«Ruyi venv-gnu-upstream» [test@arch coremark]$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=224540f496b90bf26481e95b560d31961bd03eba, for GNU/Linux 4.15.0, with debug_info, not stripped

```

5. Send file to DuoS

**Command:**
```bash
scp coremark.exe debian@10.42.0.1:
```

**Result:**
```bash
[test@arch coremark]$ scp coremark.exe debian@10.42.0.1:
debian@10.42.0.1's password:
coremark.exe                                                                                                     100%   27KB   4.3MB/s   00:00

«Ruyi venv-gnu-plct» debian@duos:~$ ls | grep coremark.exe
coremark.exe
```

#### CoreMark score

```bash
«Ruyi venv-gnu-upstream» debian@duos:~$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 15565
Total time (secs): 15.565000
Iterations/Sec   : 2569.868294
Iterations       : 40000
Compiler version : GCC14.2.0
Compiler flags   : -O2   -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x25b5
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 2569.868294 / GCC14.2.0 -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
|-----------------------|-------------|--------------------------------------------------------------|
| **Iterations/Sec**    | 2569.868294 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 15565       | Total number of clock cycles                                 |
| **Total time (secs)** | 15.565000   | Total execution time in seconds                              |
| **Iterations**        | 40000       | Total number of iterations performed                         |
| **Compiler version**  | GCC14.2.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the Sophgo SG2000 SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Sophgo SG2000 SoC:

| Test Case                  | Expected Result                        | Actual Result                                                                 | Status  |
|----------------------------|----------------------------------------|-------------------------------------------------------------------------------|---------|
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `~/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20250401.0` | ✅ PASS |
| **Compiler Verification**  | GCC 14.2.0 for RISC-V architecture     | GCC 14.2.0 with rv64gc architecture, lp64d ABI                                | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                                            | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 2569.868294       | ✅ PASS |

All tests passed successfully, confirming that the GNU Upstream Toolchain works correctly on the Sophgo SG2000 SoC.
