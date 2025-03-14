# SpacemiT K1/M1 (X60) GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information
- RuyiSDK on CanMV K230 with Canaan Kendryte K230 SoC
- Testing date: March 14, 2025

### Hardware Information
- CanMV K230 board
- Canaan Kendryte K230 SoC (XuanTie C908 Core)

## Installation

### 0. Install `ruyi` (in case you haven't yet)

```bash
apt update
apt install wget xz-utils
wget https://mirror.iscas.ac.cn/ruyisdk/ruyi/testing/0.29.0-beta.20250310/ruyi.riscv64
mv ruyi.riscv64 ruyi
chmod +x ruyi
# move to your $PATH
```

### 1. Install Toolchain

**Command:**
```bash
ruyi install toolchain/gnu-plct
```

**Result:**
```bash
root@v:~# ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: usage information has already been uploaded today at 2025-03-14 13:05:43 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /root/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
--2025-03-14 15:43:39--  https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 242595644 (231M) [application/octet-stream]
Saving to: ‘/root/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz’

/root/.cache/ruyi/distfiles/RuyiSDK 100%[================================================================>] 231.36M  8.47MB/s    in 28s     

2025-03-14 15:44:07 (8.25 MB/s) - ‘/root/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz’ saved [242595644/242595644]

info: extracting RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20240324.0
info: package gnu-plct-0.20240324.0 installed to /root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0

```

This command downloaded the toolchain package (~231MB) from ISCAS mirror and installed it to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0`.

### 2. Create Virtual Environment

**Command:**
```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result:**
```bash
root@v:~# ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Friday
info: usage information has already been uploaded today at 2025-03-14 13:05:43 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-plct...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /root/venv-gnu-plct/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.

root@v:~# 

```

**Verification of created environment contents:**
```bash
root@v:~# ls ~/venv-gnu-plct
bin                                     ruyi-cache.v2.toml  sysroot.riscv64-plct-linux-gnu
meson-cross.ini                         ruyi-venv.toml      toolchain.cmake
meson-cross.riscv64-plct-linux-gnu.ini  sysroot             toolchain.riscv64-plct-linux-gnu.cmake
root@v:~# 
root@v:~# ls ~/venv-gnu-plct/bin
riscv64-plct-linux-gnu-addr2line  riscv64-plct-linux-gnu-gcc            riscv64-plct-linux-gnu-gfortran  riscv64-plct-linux-gnu-ranlib
riscv64-plct-linux-gnu-ar         riscv64-plct-linux-gnu-gcc-ar         riscv64-plct-linux-gnu-gprof     riscv64-plct-linux-gnu-readelf
riscv64-plct-linux-gnu-as         riscv64-plct-linux-gnu-gcc-nm         riscv64-plct-linux-gnu-ld        riscv64-plct-linux-gnu-size
riscv64-plct-linux-gnu-c++        riscv64-plct-linux-gnu-gcc-ranlib     riscv64-plct-linux-gnu-ld.bfd    riscv64-plct-linux-gnu-strings
riscv64-plct-linux-gnu-c++filt    riscv64-plct-linux-gnu-gcov           riscv64-plct-linux-gnu-ldd       riscv64-plct-linux-gnu-strip
riscv64-plct-linux-gnu-cc         riscv64-plct-linux-gnu-gcov-dump      riscv64-plct-linux-gnu-lto-dump  ruyi-activate
riscv64-plct-linux-gnu-cpp        riscv64-plct-linux-gnu-gcov-tool      riscv64-plct-linux-gnu-nm
riscv64-plct-linux-gnu-elfedit    riscv64-plct-linux-gnu-gdb            riscv64-plct-linux-gnu-objcopy
riscv64-plct-linux-gnu-g++        riscv64-plct-linux-gnu-gdb-add-index  riscv64-plct-linux-gnu-objdump
root@v:~# 

```

This step created a virtual environment at `/root/venv-gnu-plct/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

### 3. Activate Environment

**Command:**
```bash
. ~/venv-gnu-plct/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:
```bash
«Ruyi venv-gnu-plct» root@v~# 
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
«Ruyi venv-gnu-plct» root@v:~# riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/13.1.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20240324 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --enable-libquadmath --enable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 13.1.0 (RuyiSDK 20240324 PLCT-Sources) 
«Ruyi venv-gnu-plct» root@v:~# 

```

The output confirmed a successful installation showing:
- GCC version: 13.1.0 (RuyiSDK 20240324 PLCT-Sources)
- Target architecture: riscv64-plct-linux-gnu
- Thread model: posix
- Configured with appropriate RISC-V specific options (rv64gc architecture, lp64d ABI)

### 2. Hello World Program

**Commands:**
```bash
«Ruyi venv-gnu-plct» root@v:~# cat hello.c
#include <stdio.h>
int main(){
        printf("Hello, World!\n");
        return 0;
}
«Ruyi venv-gnu-plct» root@v:~# riscv64-plct-linux-gnu-gcc hello.c -o hello_plct
«Ruyi venv-gnu-plct» root@v:~# ./hello_plct 
Hello, World!
«Ruyi venv-gnu-plct» root@v:~# 

```

The program executed successfully and output "Hello, World!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:
```bash
«Ruyi venv-gnu-plct» root@v:~/coremark# ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: usage information has already been uploaded today at 2025-03-14 13:05:43 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

2. Modify the build configuration to use the RISC-V compiler:
```bash
«Ruyi venv-gnu-plct» root@v:~/coremark# sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

3. Build CoreMark:
```bash
«Ruyi venv-gnu-plct» root@v:~/coremark# make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
«Ruyi venv-gnu-plct» root@v:~/coremark# 

```

4. Verify the resulting binary is a RISC-V executable:
```bash
«Ruyi venv-gnu-plct» root@v:~/coremark# ls -al
total 160
drwxr-xr-x 8 root root  4096 Mar 14 15:57 .
drwx------ 8 root root  4096 Mar 14 15:57 ..
-rw-rw-r-- 1 root root  9416 May 23  2018 LICENSE.md
-rw-rw-r-- 1 root root  3678 May 23  2018 Makefile
-rw-rw-r-- 1 root root 18799 May 23  2018 README.md
drwxrwxr-x 2 root root  4096 May 23  2018 barebones
-rw-rw-r-- 1 root root 14651 May 23  2018 core_list_join.c
-rw-rw-r-- 1 root root 12503 May 23  2018 core_main.c
-rw-rw-r-- 1 root root  8097 May 23  2018 core_matrix.c
-rw-rw-r-- 1 root root  7186 May 23  2018 core_state.c
-rw-rw-r-- 1 root root  5171 May 23  2018 core_util.c
-rwxr-xr-x 1 root root 28152 Mar 14 15:57 coremark.exe
-rw-rw-r-- 1 root root  4373 May 23  2018 coremark.h
drwxrwxr-x 2 root root  4096 May 23  2018 cygwin
drwxrwxr-x 3 root root  4096 May 23  2018 docs
drwxrwxr-x 2 root root  4096 May 23  2018 linux
drwxrwxr-x 2 root root  4096 Mar 14 15:57 linux64
drwxrwxr-x 2 root root  4096 May 23  2018 simple
«Ruyi venv-gnu-plct» root@v:~/coremark# file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=2ec45ce18cb640e991e7f27da56169eaa1652368, for GNU/Linux 4.15.0, with debug_info, not stripped
«Ruyi venv-gnu-plct» root@v:~/coremark# 

```

5. CoreMark score

```bash
«Ruyi venv-gnu-plct» root@v:~/coremark# ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 20161
Total time (secs): 20.161000
Iterations/Sec   : 5456.078568
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
CoreMark 1.0 : 5456.078568 / GCC13.1.0 -O2   -lrt / Heap
«Ruyi venv-gnu-plct» root@v:~/coremark# 

```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
| --------------------- | ----------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 5456.078568 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 20161       | Total number of clock cycles                                 |
| **Total time (secs)** | 20.161      | Total execution time in seconds                              |
| **Iterations**        | 110000      | Total number of iterations performed                         |
| **Compiler version**  | GCC13.1.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the Canaan Kendryte K230 SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Canaan Kendryte K230 SoC:

| Test Case                  | Expected Result                        | Actual Result                                                                 | Status |
| -------------------------- | -------------------------------------- | ----------------------------------------------------------------------------- | ------ |
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0` | ✅ PASS |
| **Compiler Verification**  | GCC 13.1.0 for RISC-V architecture     | GCC 13.1.0 with rv64gc architecture, lp64d ABI                                | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                                            | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5456.078568       | ✅ PASS |

All tests passed successfully, confirming that the GNU Toolchain works correctly on the Canaan Kendryte K230 SoC.