# Sophgo CV1800B GNU Toolchain (gnu-upstream) Test Report

## Environment

### System Information

- RuyiSDK on MilkV Duo with Sophgo CV1800B SoC
- Testing date: March 16, 2025

### Hardware Information

- MilkV Duo
- Sophgo CV1800B SoC

## Installation

### 1. Install Toolchain

**Command:**

```
ruyi install toolchain/gnu-upstream
```

**Result：**

```shell
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Tuesday
info: the next upload will happen anytime ruyi is executed between 2025-03-18 08:00:00 +0800 and 2025-03-19 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20231212-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz to /home/xjx/.cache/ruyi/distfiles/RuyiSDK-20231212-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  232M  100  232M    0     0  3553k      0  0:01:06  0:01:06 --:--:-- 3514k
info: extracting RuyiSDK-20231212-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz for package gnu-upstream-0.20231212.0
info: package gnu-upstream-0.20231212.0 installed to /home/xjx/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20231212.0

```

### 2. Create Virtual Environment

**Command:**

```
ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream
```

**Result:**

```shel
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Tuesday
info: the next upload will happen anytime ruyi is executed between 2025-03-18 08:00:00 +0800 and 2025-03-19 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-upstream...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/xjx/duo/venv-gnu-upstream/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.

```

**Verification of created environment contents:**

```shell
xjx@xjx-virtual-machine:~/duo$ ls venv-gnu-upstream/
bin
meson-cross.ini
meson-cross.riscv64-unknown-linux-gnu.ini
ruyi-cache.v2.toml
ruyi-venv.toml
sysroot
sysroot.riscv64-unknown-linux-gnu
toolchain.cmake
toolchain.riscv64-unknown-linux-gnu.cmake

xjx@xjx-virtual-machine:~/duo$ ls venv-gnu-upstream/bin/
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

### 3. Activate Environment

**Command:**

```shell
. venv-gnu-upstream/bin/ruyi_activate
```

**Result:** The prompt changed to indicate active environment:

```
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo$ 
```

This initialized the environment and provided access to all cross-compilation tools.

## Tests & Results

### 1. Compiler Version Check

**Command:**

```shell
riscv64-unknown-linux-gnu-gcc -v
```

**Result:**

```
Using built-in specs.
COLLECT_GCC=/home/xjx/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20231212.0/bin/riscv64-unknown-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/xjx/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20231212.0/bin/../libexec/gcc/riscv64-unknown-linux-gnu/13.2.0/lto-wrapper
Target: riscv64-unknown-linux-gnu
Configured with: /work/riscv64-unknown-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=x86_64-build_pc-linux-gnu --target=riscv64-unknown-linux-gnu --prefix=/opt/ruyi/riscv64-unknown-linux-gnu --exec_prefix=/opt/ruyi/riscv64-unknown-linux-gnu --with-sysroot=/opt/ruyi/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20231212 Upstream-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --enable-libquadmath --enable-libquadmath-support --disable-libmpx --with-gmp=/work/riscv64-unknown-linux-gnu/buildtools --with-mpfr=/work/riscv64-unknown-linux-gnu/buildtools --with-mpc=/work/riscv64-unknown-linux-gnu/buildtools --with-isl=/work/riscv64-unknown-linux-gnu/buildtools --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-long-long
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

```
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo$ cat hello.c
#include <stdio.h>
int main(){
	printf("Hello World\n");
	return 0;
}

xjx@xjx-virtual-machine:~/duo$ scp hello root@192.168.42.1:/root/
root@192.168.42.1's password: 
hello                                                                                100% 3904KB   2.4MB/s   00:01    
xjx@xjx-virtual-machine:~/duo$ ssh root@192.168.42.1
root@192.168.42.1's password: 
[root@milkv-duo]~# ls
hello
[root@milkv-duo]~# ./hello
Hello World

```

### 3. CoreMark Benchmark (-O2 -lrt)

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:

```
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo$ mkdir coremark
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo$ cd coremark/
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo/coremark$  ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Tuesday
info: the next upload will happen anytime ruyi is executed between 2025-03-18 08:00:00 +0800 and 2025-03-19 08:00:00 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/xjx/.cache/ruyi/distfiles/coremark-1.01.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  391k  100  391k    0     0  1186k      0 --:--:-- --:--:-- --:--:-- 1183k
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo/coremark$ ls
barebones         core_main.c  core_matrix.c  core_util.c  docs        linux    Makefile   simple
core_list_join.c  coremark.h   core_state.c   cygwin       LICENSE.md  linux64  README.md

```

2. Modify the build configuration to use the RISC-V compiler:

```
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo/coremark$ cat linux64/core_portme.mak | grep gcc
CC = gcc
LD		= gcc
# E.g. generate profile guidance files. Sample PGO generation for gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo/coremark$ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo/coremark$ cat linux64/core_portme.mak | grep gcc
CC = riscv64-unknown-linux-gnu-gcc
LD		= riscv64-unknown-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-unknown-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:

```

3. Build CoreMark:

```
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo/coremark$ make PORT_DIR=linux64 XCFLAGS="-static" link
CFLAGS : -O2 -Ilinux64 -I. -DFLAGS_STR="-O2 -static -lrt" -DITERATIONS=0
XCFLAGS: -static
riscv64-unknown-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -static  -lrt"\" -DITERATIONS=0 -static core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:

```
«Ruyi venv-gnu-upstream» xjx@xjx-virtual-machine:~/duo/coremark$ file coremark.exe 
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), statically linked, BuildID[sha1]=7d2f15bff2ca22fb44cca2e4e3b6b6810cbe2e3b, for GNU/Linux 4.15.0, with debug_info, not stripped
```

#### CoreMark score

```shell
xjx@xjx-virtual-machine:~/duo/coremark$ scp coremark.exe root@192.168.42.1:/root/
root@192.168.42.1's password: 
coremark.exe                                                                         100% 3934KB   2.8MB/s   00:01    
xjx@xjx-virtual-machine:~/duo/coremark$ ssh root@192.168.42.1
root@192.168.42.1's password: 
[root@milkv-duo]~# ls
coremark.exe  hello
[root@milkv-duo]~# ./coremark.exe 
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 14927
Total time (secs): 14.927000
Iterations/Sec   : 2009.780934
Iterations       : 30000
Compiler version : GCC13.2.0
Compiler flags   : -O2 -static  -lrt
Memory location  : Please put data memory location here
			(e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x5275
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 2009.780934 / GCC13.2.0 -O2 -static  -lrt / Heap
[root@milkv-duo]~# 

```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
| --------------------- | ----------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 2009.780934 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 14927       | Total number of clock cycles                                 |
| **Total time (secs)** | 14.927000   | Total execution time in seconds                              |
| **Iterations**        | 30000       | Total number of iterations performed                         |
| **Compiler version**  | GCC13.2.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the Sophgo CV1800B SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Sophgo CV1800B SoC:

| Test Case                  | Expected Result                        | Actual Result                                                | Status |
| -------------------------- | -------------------------------------- | ------------------------------------------------------------ | ------ |
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `installed to /home/xjx/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20231212.0` | ✅ PASS |
| **Compiler Verification**  | GCC 13.2.0 for RISC-V architecture     | GCC 13.2.0 with rv64gc architecture, lp64d ABI               | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                           | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 2009.780934 | ✅ PASS |

All tests passed successfully, confirming that the GNU Upstream Toolchain works correctly on the Sophgo CV1800B SoC.