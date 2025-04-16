# Sophgo CV1800B GNU Toolchain (gnu-upstream) Test Report

## Environment

### System Information

- RuyiSDK on Milk-V Duo with Sophgo CV1800B SoC
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
ruyi install toolchain/gnu-upstream
```

**Result:**

```bash
(base) saicogn-rk@saicogn-rk:~$ ruyi install toolchain/gnu-upstream
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz to /home/saicogn-rk/.cache/ruyi/distfiles/RuyiSDK-20250401-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  164M  100  164M    0     0  8463k      0  0:00:19  0:00:19 --:--:-- 12.5M
info: extracting RuyiSDK-20250401-Upstream-Sources-riscv64-unknown-linux-gnu.tar.xz for package gnu-upstream-0.20250401.0
info: package gnu-upstream-0.20250401.0 installed to /home/saicogn-rk/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20250401.0
```

This command downloaded the toolchain package (~164MB) from ISCAS mirror and installed it to `/root/.local/share/ruyi/binaries/riscv64/gnu-upstream-0.20250401.0`.

### 2. Create Virtual Environment

**Command:**

```bash
ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream-250401
```

**Result:**

```bash
(base) saicogn-rk@saicogn-rk:~$ ruyi venv -t toolchain/gnu-upstream generic venv-gnu-upstream-250401
info: Creating a Ruyi virtual environment at venv-gnu-upstream-250401...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/saicogn-rk/venv-gnu-upstream-250401/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**

```bash

(base) saicogn-rk@saicogn-rk:~$ ls ~/venv-gnu-upstream-250401/
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

(base) saicogn-rk@saicogn-rk:~$ ls ~/venv-gnu-upstream-250401/bin/
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

This step created a virtual environment at `/root/'your user'/venv-gnu-upstream-250401/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-unknown-linux-gnu-`.

### 3. Activate Environment

**Command:**

```bash
. ~/venv-gnu-upstream-250401/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:

```bash
(base) saicogn-rk@saicogn-rk:~$ . ./venv-gnu-upstream-250401/bin/ruyi-activate 
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ 
```

This initialized the environment and provided access to all cross-compilation tools.

To exit virtual environment:

```bash
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ ruyi-deactivate 
(base) saicogn-rk@saicogn-rk:~$ 
```

## Tests & Results

### 1. Compiler Version Check

**Command:**

```shell
riscv64-unknown-linux-gnu-gcc -v
```

**Result:**

```bash
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ riscv64-unknown-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/saicogn-rk/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20250401.0/bin/riscv64-unknown-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/saicogn-rk/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20250401.0/bin/../libexec/gcc/riscv64-unknown-linux-gnu/14.2.0/lto-wrapper
Target: riscv64-unknown-linux-gnu
Configured with: /work/riscv64-unknown-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=x86_64-build_pc-linux-gnu --target=riscv64-unknown-linux-gnu --prefix=/opt/ruyi/riscv64-unknown-linux-gnu --exec_prefix=/opt/ruyi/riscv64-unknown-linux-gnu --with-sysroot=/opt/ruyi/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250401 Upstream-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/riscv64-unknown-linux-gnu/buildtools --with-mpfr=/work/riscv64-unknown-linux-gnu/buildtools --with-mpc=/work/riscv64-unknown-linux-gnu/buildtools --with-isl=/work/riscv64-unknown-linux-gnu/buildtools --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --disable-multilib --with-local-prefix=/opt/ruyi/riscv64-unknown-linux-gnu/riscv64-unknown-linux-gnu/sysroot --enable-long-long
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
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ vim Hello.c
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ cat Hello.c 
#include <stdio.h>

int main(void)
{
    printf("Hello World!\r\n");
    return 0;
}

«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ riscv64-unknown-linux-gnu-gcc Hello.c -o Hello_uptsream_static -static

«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ file Hello_upstream_static 
Hello_static: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (GNU/Linux), statically linked, BuildID[sha1]=9de088294bc3fbc0009cb5da94f20a711f714bac, for GNU/Linux 4.15.0, with debug_info, not stripped

«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ scp Hello_upstream_static root@192.168.42.1:~

«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ ssh root@192.168.42.1
root@192.168.42.1's password: 

[root@milkv-duo]~# ./Hello_upstream_static
Hello World!
```

The program executed successfully and output "Hello World!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark (-O2 -lrt)

**Commands and Results:**

#### 1. Extract the CoreMark package with Ruyi

```bash
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~$ mkdir coremark-upstream-250401 && cd coremark-upstream-250401
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$


«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ ruyi extract coremark
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory

«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ ls
barebones         core_main.c  core_matrix.c  core_util.c  docs        linux    Makefile   simple
core_list_join.c  coremark.h   core_state.c   cygwin       LICENSE.md  linux64  README.md
```

#### 2. Modify the build configuration to use the RISC-V compiler

```bash
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ sed -i 's/\bgcc\b/riscv64-unknown-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ grep "gcc" ./linux64/core_portme.mak 
CC = riscv64-unknown-linux-gnu-gcc
LD    = riscv64-unknown-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-unknown-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

#### 3. CoreMark BenchMark with Default Flag (-O -lrt -static)

**Build CoreMark:**

```bash
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ make PORT_DIR=linux64 XCFLAGS="-static" link
riscv64-unknown-linux-gnu-gcc -O2 -Ilinux64/ -I. -DFLAGS_STR=\""-O2 -static  -lrt"\" -DITERATIONS=0 -static core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64//core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

#### 4. Verify the resulting binary is a RISC-V executable

```bash
«Ruyi venv-gnu-upstream-250401» (base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ file coremark.exe 
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, version 1 (GNU/Linux), statically linked, BuildID[sha1]=513ffefbecfba45e667d4fbb331475512fd3b13f, for GNU/Linux 4.15.0, with debug_info, not stripped
```

**Send to Milk-V Duo:**

```bash
(base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ scp coremark.exe root@192.168.42.1:/root/
```

#### 5. CoreMark score

```bash

(base) saicogn-rk@saicogn-rk:~/coremark-upstream-250401$ ssh root@192.168.42.1
root@192.168.42.1's password: 

[root@milkv-duo]~# ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 14738
Total time (secs): 14.738000
Iterations/Sec   : 2035.554349
Iterations       : 30000
Compiler version : GCC14.2.0
Compiler flags   : -O2 -static  -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x5275
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 2035.554349 / GCC14.2.0 -O2 -static  -lrt / Heap
```

##### 6. CoreMark Results

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value            | Description                                                  |
| --------------------- | ---------------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 2035.554349      | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 14738            | Total number of clock cycles                                 |
| **Total time (secs)** | 14.738000        | Total execution time in seconds                              |
| **Iterations**        | 30000            | Total number of iterations performed                         |
| **Compiler version**  | GCC14.2.0        | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt -static | Compilation flags used                                       |
| **Memory location**   | Heap             | Where data is stored during execution                        |

These results demonstrate good performance of the Sophgo CV1800B SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Sophgo CV1800B SoC:

| Test Case                  | Expected Result                        | Actual Result                                                | Status |
| -------------------------- | -------------------------------------- | ------------------------------------------------------------ | ------ |
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `installed to /home/xjx/.local/share/ruyi/binaries/x86_64/gnu-upstream-0.20250401.0` | ✅ PASS |
| **Compiler Verification**  | GCC 14.2.0 for RISC-V architecture     | GCC 14.2.0 with rv64gc architecture, lp64d ABI               | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                           | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 2035.554349 | ✅ PASS |

All tests passed successfully, confirming that the GNU Upstream Toolchain works correctly on the Sophgo CV1800B SoC.
