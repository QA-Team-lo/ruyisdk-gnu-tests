# Sophgo CV1800B GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information

- RuyiSDK on Mil-V Duo with Sophgo CV1800B SoC
- Board OS: Milk-V Duo official BuildRoot image (Releases V1.1.4)
- Host Environment: Ubuntu22.04 LTS x86_64
- Testing date: April 16, 2025

### Hardware Information

- Milk-V Duo
- Sophgo CV1800B SoC

## Installation

### 0. Install ruyi

```bash
sudo apt update
sudo apt install wget xz-utils make
wget https://mirror.iscas.ac.cn/ruyisdk/ruyi/releases/0.31.0/ruyi.amd64
chmod +x ./ruyi.amd64
sudo cp -v ruyi.amd64 /usr/local/bin/ruyi

(base) saicogn-rk@saicogn-rk:~$ ruyi -h
usage: ruyi [-h] [-V] [--porcelain] {admin,list,extract,install,i,device,venv,news,update,telemetry,self,config,version} ...

RuyiSDK Package Manager 0.31.0

options:
  -h, --help            show this help message and exit
  -V, --version         Print version information
  --porcelain           Give the output in a machine-friendly format if applicable

subcommands:
  {admin,list,extract,install,i,device,venv,news,update,telemetry,self,config,version}
    admin               (NOT FOR REGULAR USERS) Subcommands for managing Ruyi repos
    list                List available packages in configured repository
    extract             Fetch package(s) then extract to current directory
    install (i)         Install package from configured repository
    device              Manage devices
    venv                Generate a virtual environment adapted to the chosen toolchain and profile
    news                List and read news items from configured repository
    update              Update RuyiSDK repo and packages
    telemetry           Manage your telemetry preferences
    self                Manage this Ruyi installation
    config              Manage Ruyi's config options
    version             Print version information

(base) saicogn-rk@saicogn-rk:~$ ruyi update
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Compressing objects: 100% (5/5), done.
Total 10 (delta 3), reused 8 (delta 3), pack-reused 0 (from 0)
transferring objects ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   0% -:--:--
processing deltas    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00
(base) saicogn-rk@saicogn-rk:~$ 

(base) saicogn-rk@saicogn-rk:~$ ruyi self clean --distfiles --installed-pkgs
info: removing installed packages
info: removing downloaded distfiles
```

### 1. Install Toolchain

**Command:**

```bash
ruyi install toolchain/gnu-plct
```

**Result:**

```bash
(base) saicogn-rk@saicogn-rk:~$ ruyi install toolchain/gnu-plct
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz to /home/saicogn-rk/.cache/ruyi/distfiles/RuyiSDK-20250401-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  159M  100  159M    0     0  10.0M      0  0:00:15  0:00:15 --:--:-- 13.4M
info: extracting RuyiSDK-20250401-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20250401.0
info: package gnu-plct-0.20250401.0 installed to /home/saicogn-rk/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250401.0
```

This command downloaded the toolchain package (~159MB) from ISCAS mirror and installed it to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250401.0`.

### 2. Create Virtual Environment

**Command:**

```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct-250401
```

**Result:**

```bash
(base) saicogn-rk@saicogn-rk:~$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct-250401
info: Creating a Ruyi virtual environment at venv-gnu-plct-250401...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/saicogn-rk/venv-gnu-plct-250401/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**

```bash

(base) saicogn-rk@saicogn-rk:~$ ls ~/venv-gnu-plct-250401/
bin                                     sysroot
meson-cross.ini                         sysroot.riscv64-plct-linux-gnu
meson-cross.riscv64-plct-linux-gnu.ini  toolchain.cmake
ruyi-cache.v2.toml                      toolchain.riscv64-plct-linux-gnu.cmake
ruyi-venv.toml

(base) saicogn-rk@saicogn-rk:~$ ls ~/venv-gnu-plct-250401/bin
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

This step created a virtual environment at `/home/'your user'/venv-gnu-plct-250401/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

### 3. Activate Virtual Environment

**Command:**

```bash
. ~/venv-gnu-plct-250401/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:

```bash
(base) saicogn-rk@saicogn-rk:~$ . ./venv-gnu-plct-250401/bin/ruyi-activate 
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ 
```

This initialized the environment and provided access to all cross-compilation tools.

To exit virtual environment:

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ ruyi-deactivate 
(base) saicogn-rk@saicogn-rk:~$ 
```

## Tests & Results

### 1. Compiler Version Check

**Command：**

```shell
riscv64-plct-linux-gnu-gcc -v
```

**Result:**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/saicogn-rk/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250401.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/saicogn-rk/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250401.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/14.1.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=x86_64-build_pc-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250401 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/riscv64-plct-linux-gnu/buildtools --with-mpfr=/work/riscv64-plct-linux-gnu/buildtools --with-mpc=/work/riscv64-plct-linux-gnu/buildtools --with-isl=/work/riscv64-plct-linux-gnu/buildtools --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --disable-multilib --with-local-prefix=/opt/ruyi/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 14.1.0 (RuyiSDK 20250401 PLCT-Sources) 
```

The output confirmed a successful installation showing:

- GCC version: 14.1.0 (RuyiSDK 20250401 PLCT-Sources)
- Target architecture: riscv64-plct-linux-gnu
- Thread model: posix
- Configured with appropriate RISC-V specific options (rv64gc architecture, lp64d ABI)

### 2. Hello World Program

**Commands：**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ vim Hello.c
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ cat Hello.c 
#include <stdio.h>

int main(void)
{
    printf("Hello World!\r\n");
    return 0;
}

«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ riscv64-plct-linux-gnu-gcc Hello.c -o Hello_plct_static -static

«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ file Hello_plct_static 
Hello_plct_static: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), statically linked, BuildID[sha1]=760669cf73c43975ff1149b11b2b72ef00fec13f, for GNU/Linux 4.15.0, with debug_info, not stripped

«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ scp Hello_plct_static root@192.168.42.1:~

«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ ssh root@192.168.42.1
root@192.168.42.1's password: 

[root@milkv-duo]~# ./Hello_plct_static
Hello World!
```

The program executed successfully and output "Hello World!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark (-O2 -lrt -static)

#### 1. Extract the CoreMark package with Ruyi

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ mkdir coremark-plct-250401 && cd coremark-plct-250401
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ 

«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ ruyi extract coremark
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/saicogn-rk/.cache/ruyi/distfiles/coremark-1.01.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  391k  100  391k    0     0   639k      0 --:--:-- --:--:-- --:--:--  639k
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

#### 2. Modify the build configuration to use the RISC-V compiler

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' ./linux64/core_portme.mak

«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ grep "gcc" ./linux64/core_portme.mak
CC = riscv64-plct-linux-gnu-gcc
LD    = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

#### 3. CoreMark BenchMark with Default Flag (-O -lrt -static)

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ make PORT_DIR=linux64 XCFLAGS="-static" link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -static  -lrt"\" -DITERATIONS=0 -static core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

#### 4. Verify the resulting binary is a RISC-V executable

```shell
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ file coremark.exe 
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, version 1 (SYSV), statically linked, BuildID[sha1]=dfde058df55e72a5a841cd893b17a4512099d0e6, for GNU/Linux 4.15.0, with debug_info, not stripped
[root@milkv-duo]~#
```

**Send to Milk-V Duo:**

```bash
(base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ scp coremark.exe root@192.168.42.1:/root/
```

#### 5. CoreMark score

```bash
(base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ ssh root@192.168.42.1
root@192.168.42.1's password: 

[root@milkv-duo]~# ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 14737
Total time (secs): 14.737000
Iterations/Sec   : 2035.692475
Iterations       : 30000
Compiler version : GCC14.1.0
Compiler flags   : -O2 -static  -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x5275
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 2035.692475 / GCC14.1.0 -O2 -static  -lrt / Heap
```

#### 6. CoreMark Results

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value            | Description                                                  |
| --------------------- | ---------------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 2035.692475      | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 14737            | Total number of clock cycles                                 |
| **Total time (secs)** | 14.737000        | Total execution time in seconds                              |
| **Iterations**        | 30000            | Total number of iterations performed                         |
| **Compiler version**  | GCC14.1.0        | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt -static | Compilation flags used                                       |
| **Memory location**   | Heap             | Where data is stored during execution                        |

These results demonstrate good performance of the Sophgo CV1800B SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Sophgo CV1800B SoC:

| Test Case                  | Expected Result                        | Actual Result                                                | Status |
| -------------------------- | -------------------------------------- | ------------------------------------------------------------ | ------ |
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `/home/xjx/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250301.0` | ✅ PASS |
| **Compiler Verification**  | GCC 14.1.0 for RISC-V architecture     | GCC 14.1.0 with rv64gc architecture, lp64d ABI               | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                           | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 2035.692475 | ✅ PASS |

All tests passed successfully, confirming that the GNU Upstream Toolchain works correctly on the Sophgo CV1800B SoC.
