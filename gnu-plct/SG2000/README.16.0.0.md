# Sophgo SG2000 GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information
- RuyiSDK on MilkV DuoS with Sophgo SG2000 SoC
- Testing date: June 26, 2025

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
info: the next upload will happen anytime ruyi is executed between 2025-06-28 00:00:00 +0000 and 2025-06-29 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /home/debian/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
--2025-06-26 09:38:52--  https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 302 Moved Temporarily
Location: https://fast-mirror.isrc.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz [following]
--2025-06-26 09:38:53--  https://fast-mirror.isrc.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
Resolving fast-mirror.isrc.ac.cn (fast-mirror.isrc.ac.cn)... 210.73.43.2
Connecting to fast-mirror.isrc.ac.cn (fast-mirror.isrc.ac.cn)|210.73.43.2|:443... connected.
HTTP request sent, awaiting response... 206 Partial Content
Length: 376886364 (359M), 210015625 (200M) remaining [application/octet-stream]
Saving to: '/home/debian/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz'

/home/debian/.cache/ruyi/distfiles/Ruy 100%[++++++++++++++++++++++++++++++++++==========================================>] 359.43M  2.41MB/s    in 72s

2025-06-26 09:40:05 (2.80 MB/s) - '/home/debian/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz' saved [376886364/376886364]

info: extracting RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20250615.0

info: package gnu-plct-0.20250615.0 installed to /home/debian/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0
```

### 2. Create Virtual Environment

**Command:**
```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result:**
```bash
debian@duos:~$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Saturday
info: the next upload will happen anytime ruyi is executed between 2025-06-28 00:00:00 +0000 and 2025-06-29 00:00:00 +0000
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
debian@duos:~$ ls ~/venv-gnu-plct/bin/
riscv64-plct-linux-gnu-addr2line  riscv64-plct-linux-gnu-gcc-ranlib       riscv64-plct-linux-gnu-gprof                 riscv64-plct-linux-gnu-nm
riscv64-plct-linux-gnu-ar         riscv64-plct-linux-gnu-gcov             riscv64-plct-linux-gnu-gprofng               riscv64-plct-linux-gnu-objcopy
riscv64-plct-linux-gnu-as         riscv64-plct-linux-gnu-gcov-dump        riscv64-plct-linux-gnu-gprofng-archive       riscv64-plct-linux-gnu-objdump
riscv64-plct-linux-gnu-c++        riscv64-plct-linux-gnu-gcov-tool        riscv64-plct-linux-gnu-gprofng-collect-app   riscv64-plct-linux-gnu-ranlib
riscv64-plct-linux-gnu-c++filt    riscv64-plct-linux-gnu-gdb              riscv64-plct-linux-gnu-gprofng-display-html  riscv64-plct-linux-gnu-readelf
riscv64-plct-linux-gnu-cc         riscv64-plct-linux-gnu-gdb-add-index    riscv64-plct-linux-gnu-gprofng-display-src   riscv64-plct-linux-gnu-size
riscv64-plct-linux-gnu-cpp        riscv64-plct-linux-gnu-gfortran         riscv64-plct-linux-gnu-gprofng-display-text  riscv64-plct-linux-gnu-strings
riscv64-plct-linux-gnu-elfedit    riscv64-plct-linux-gnu-gp-archive       riscv64-plct-linux-gnu-gstack                riscv64-plct-linux-gnu-strip
riscv64-plct-linux-gnu-g++        riscv64-plct-linux-gnu-gp-collect-app   riscv64-plct-linux-gnu-ld                    ruyi-activate
riscv64-plct-linux-gnu-gcc        riscv64-plct-linux-gnu-gp-display-html  riscv64-plct-linux-gnu-ld.bfd
riscv64-plct-linux-gnu-gcc-ar     riscv64-plct-linux-gnu-gp-display-src   riscv64-plct-linux-gnu-ldd
riscv64-plct-linux-gnu-gcc-nm     riscv64-plct-linux-gnu-gp-display-text  riscv64-plct-linux-gnu-lto-dump
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
debian@duos:~$ . ~/venv-gnu-plct/bin/ruyi-activate
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
COLLECT_GCC=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/debian/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/16.0.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250615 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --enable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 16.0.0 20250512 (experimental) (RuyiSDK 20250615 PLCT-Sources)
```

The output confirmed a successful installation showing:
- GCC version: 16.0.0 (RuyiSDK 20250615 PLCT-Sources)
- Target architecture: riscv64-plct-linux-gnu
- Thread model: posix
- Configured with appropriate RISC-V specific options (rv64gc architecture, lp64d ABI)

### 2. Hello World Program

**Commands:**
```bash
«Ruyi venv-gnu-plct» debian@duos:~$ cat hello.c
#include <stdio.h>

int main() {
    printf("Hello, world!\n");
    return 0;
}
«Ruyi venv-gnu-plct» debian@duos:~$ riscv64-plct-linux-gnu-gcc hello.c -o hello && ./hello
Hello, world!

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
«Ruyi venv-gnu-plct» debian@duos:~$ mkdir coremark && cd coremark
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Saturday
info: the next upload will happen anytime ruyi is executed between 2025-06-28 00:00:00 +0000 and 2025-06-29 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/debian/.cache/ruyi/distfiles/coremark-1.01.tar.gz
--2025-06-26 10:25:11--  https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 302 Moved Temporarily
Location: https://fast-mirror.isrc.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz [following]
--2025-06-26 10:25:12--  https://fast-mirror.isrc.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz
Resolving fast-mirror.isrc.ac.cn (fast-mirror.isrc.ac.cn)... 210.73.43.2
Connecting to fast-mirror.isrc.ac.cn (fast-mirror.isrc.ac.cn)|210.73.43.2|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 401208 (392K) [application/octet-stream]
Saving to: '/home/debian/.cache/ruyi/distfiles/coremark-1.01.tar.gz'

                   /home/debian/.cache   0%[                                                                        /home/debian/.cache/ruyi/distfiles/cor 100%[============================================================================>] 391.80K  2.01MB/s    in 0.2s

2025-06-26 10:25:12 (2.01 MB/s) - '/home/debian/.cache/ruyi/distfiles/coremark-1.01.tar.gz' saved [401208/401208]

info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ ls
LICENSE.md  README.md  core_list_join.c  core_matrix.c  core_util.c  cygwin  linux    simple
Makefile    barebones  core_main.c       core_state.c   coremark.h   docs    linux64
```

2. Modify the build configuration to use the RISC-V compiler:

**Command:**
```bash
sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

**Result:**
```bash
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ cat linux64/core_portme.mak | grep gcc
CC = gcc
LD              = gcc
# E.g. generate profile guidance files. Sample PGO generation for gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ cat linux64/core_portme.mak | grep gcc
CC = riscv64-plct-linux-gnu-gcc
LD              = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
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
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:

**Command:**
```bash
file coremark.exe
```

**Result:**
```bash
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=865e58d260b2b3190200b6f5d1e22ca967733977, for GNU/Linux 4.15.0, with debug_info, not stripped
```

#### Cross Compile

**System and Hardware Information:**
- OS: Arch Linux x86_64
- Kernel: Linux 6.15.3-arch1-1

**Commands and Results:**

1. Install toolchain

**Command:**
```bash
ruyi install toolchain/gnu-plct
```

**Result:**
```bash
[mitchell@nekoarch tmp]$ ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Tuesday
info: the next upload will happen anytime ruyi is executed between 2025-07-01 08:00:00 +0800 and 2025-07-02 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz to /home/mitchell/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz
curl: option --ftp-pasv: is unknown
curl: try 'curl --help' for more information
warn: failed to fetch distfile: command 'curl -L --connect-timeout 60 --ftp-pasv -o /home/mitchell/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz' returned 2
info: retrying download (2 of 3 times)
curl: option --ftp-pasv: is unknown
curl: try 'curl --help' for more information
warn: failed to fetch distfile: command 'curl -L --connect-timeout 60 --ftp-pasv -o /home/mitchell/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz' returned 2
info: retrying download (3 of 3 times)
curl: option --ftp-pasv: is unknown
curl: try 'curl --help' for more information
warn: failed to fetch distfile: command 'curl -L --connect-timeout 60 --ftp-pasv -o /home/mitchell/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz' returned 2
fatal error: failed to fetch '/home/mitchell/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz': all source URLs have failed

Downloads can fail for a multitude of reasons, most of which should not and
cannot be handled by Ruyi. For your convenience though, please check if any
of the following common failure modes apply to you, and take actions
accordingly if one of them turns out to be the case:

* Basic connectivity problems
    - is the gateway reachable?
    - is common websites reachable?
    - is there any DNS pollution?
* Organizational and/or ISP restrictions
    - is there a firewall preventing Ruyi traffic?
    - is your ISP blocking access to the source website?
* Volatile upstream
    - is the recorded link dead? (Please raise a Ruyi issue for a fix!)
```

**Test Failed**

#### CoreMark score

```bash
«Ruyi venv-gnu-plct» debian@duos:~/coremark$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 14579
Total time (secs): 14.579000
Iterations/Sec   : 2743.672406
Iterations       : 40000
Compiler version : GCC16.0.0 20250512 (experimental)
Compiler flags   : -O2   -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x25b5
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 2743.672406 / GCC16.0.0 20250512 (experimental) -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
|-----------------------|-------------|--------------------------------------------------------------|
| **Iterations/Sec**    | 2743.672406 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 14579       | Total number of clock cycles                                 |
| **Total time (secs)** | 14.579000   | Total execution time in seconds                              |
| **Iterations**        | 40000       | Total number of iterations performed                         |
| **Compiler version**  | GCC16.0.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the Sophgo SG2000 SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Sophgo SG2000 SoC:

| Test Case                      | Expected Result                               | Actual Result                                                             | Status  |
|--------------------------------|-----------------------------------------------|---------------------------------------------------------------------------|---------|
| **Toolchain Installation**     | Successfully installed toolchain              | Installed to `~/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250401.0` | ✅ PASS |
| **Cross Compile Installation** | Successfully installed toolchain on ArchLinux | Installation Failed                                                       | ❎ FAIL |
| **Compiler Verification**      | GCC 16.0.0 for RISC-V architecture            | GCC 16.0.0 with rv64gc architecture, lp64d ABI                            | ✅ PASS |
| **Hello World Test**           | Successful compilation and execution          | Successfully compiled and executed                                        | ✅ PASS |
| **CoreMark Benchmark**         | Successfully compile and run benchmark        | Successfully compiled and completed benchmark with score of 2743.672406   | ✅ PASS |

Cross-compilation installation encountered failures, while all other test cases succeeded, confirming the GNU Toolchain's core functionality on the Sophgo SG2000 SoC.
