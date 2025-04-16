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
sudo wget https://mirror.iscas.ac.cn/ruyisdk/ruyi/releases/0.31.0/ruyi.riscv64
sudo cp -v ruyi.riscv64 /usr/local/bin/ruyi  # Copy to your $PATH
sudo chmod +x /usr/local/bin/ruyi

user@milkv:~$ ruyi -h
usage: ruyi [-h] [-V] [--porcelain]
            {admin,list,extract,install,i,device,venv,news,update,telemetry,self,config,version} ...

RuyiSDK Package Manager 0.32.0-alpha.20250408

options:
  -h, --help            show this help message and exit
  -V, --version         Print version information
  --porcelain           Give the output in a machine-friendly format if
                        applicable

subcommands:
  {admin,list,extract,install,i,device,venv,news,update,telemetry,self,config,version}
    admin               (NOT FOR REGULAR USERS) Subcommands for managing Ruyi
                        repos
    list                List available packages in configured repository
    extract             Fetch package(s) then extract to current directory
    install (i)         Install package from configured repository
    device              Manage devices
    venv                Generate a virtual environment adapted to the chosen
                        toolchain and profile
    news                List and read news items from configured repository
    update              Update RuyiSDK repo and packages
    telemetry           Manage your telemetry preferences
    self                Manage this Ruyi installation
    config              Manage Ruyi's config options
    version             Print version information
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
info: the next upload will happen anytime ruyi is executed between 2025-04-17 00:00:00 +0000 and 2025-04-18 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /home/user/.cache/ruyi/distfiles/RuyiSDK-20250401-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
--2025-04-15 17:42:38--  https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 161794864 (154M) [application/octet-stream]
Saving to: ‘/home/user/.cache/ruyi/distfiles/RuyiSDK-20250401-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz’

/home/user/.cache/r 100%[===================>] 154.30M  11.0MB/s    in 15s

2025-04-15 17:42:52 (10.6 MB/s) - ‘/home/user/.cache/ruyi/distfiles/RuyiSDK-20250401-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz’ saved [161794864/161794864]

info: extracting RuyiSDK-20250401-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20250401.0
info: package gnu-plct-0.20250401.0 installed to /home/user/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250401.0
```

This command downloaded the toolchain package (~231MB) from ISCAS mirror and installed it to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250401.0`.

### 2. Create Virtual Environment

**Command:**

```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result:**

```bash
user@milkv:~$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct-250401
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-04-17 00:00:00 +0000 and 2025-04-18 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-plct-250401...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/user/venv-gnu-plct-250401/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**

```bash
user@milkv:~$ ls ~/venv-gnu-plct-250401/
bin                                     sysroot
meson-cross.ini                         sysroot.riscv64-plct-linux-gnu
meson-cross.riscv64-plct-linux-gnu.ini  toolchain.cmake
ruyi-cache.v2.toml                      toolchain.riscv64-plct-linux-gnu.cmake
ruyi-venv.toml

user@milkv:~$ ls ~/venv-gnu-plct-250401/bin/
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
user@milkv:~$ . ~/venv-gnu-plct-250401/bin/ruyi-activate
«Ruyi venv-gnu-plct-250401» user@milkv:~$
```

This initialized the environment and provided access to all cross-compilation tools.

To exit virtual environment:

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~$ ruyi-deactivate 
user@milkv:~$
```

## Tests & Results

### 1. Compiler Version Check

**Command:**

```bash
riscv64-plct-linux-gnu-gcc -v
```

**Result:**

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~$ riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/user/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250401.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/user/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250401.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/14.1.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250401 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --disable-multilib --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
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

**Program:**

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~$ cat Hello.c
#include <stdio.h>

int main(void)
{
    printf("Milk-V Mars: Hello World!\r\n");
    return 0;
}
```

**Commands:**

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~$ riscv64-plct-linux-gnu-gcc Hello.c -o Hello
«Ruyi venv-gnu-plct-250401» user@milkv:~$ file Hello
Hello: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=a70cd63f829ff8a6bf5fc6989ba2c9c229dd74f2, for GNU/Linux 4.15.0, with debug_info, not stripped
«Ruyi venv-gnu-plct-250401» user@milkv:~$ ./Hello
Milk-V Mars: Hello World!
```

The program executed successfully and output "Milk-V Mars: Hello World!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark with Default Flag (-O2 -lrt)

#### 1. Extract the CoreMark package with Ruyi

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~$ mkdir coremark-plct-250401 && cd coremark-plct-250401/
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$

«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: the next upload will happen anytime ruyi is executed between 2025-04-17 00:00:00 +0000 and 2025-04-18 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/user/.cache/ruyi/distfiles/coremark-1.01.tar.gz
--2025-04-15 17:59:14--  https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz
Resolving mirror.iscas.ac.cn (mirror.iscas.ac.cn)... 124.16.138.126
Connecting to mirror.iscas.ac.cn (mirror.iscas.ac.cn)|124.16.138.126|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 401208 (392K) [application/octet-stream]
Saving to: ‘/home/user/.cache/ruyi/distfiles/coremark-1.01.tar.gz’

/home/user/.cache/r 100%[===================>] 391.80K  2.15MB/s    in 0.2s

2025-04-15 17:59:14 (2.15 MB/s) - ‘/home/user/.cache/ruyi/distfiles/coremark-1.01.tar.gz’ saved [401208/401208]

info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

#### 2. Modify the build configuration to use the RISC-V compiler

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' ./linux64/core_portme.mak

«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ grep "gcc" ./linux64/core_portme.mak
CC = riscv64-plct-linux-gnu-gcc
LD              = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

#### 3. Build CoreMark

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

#### 4. Verify the resulting binary is a RISC-V executable

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=db453fedfca2f8f9d2f6567a949c0147daf8218f, for GNU/Linux 4.15.0, with debug_info, not stripped
```

#### 5. CoreMark score

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 20741
Total time (secs): 20.741000
Iterations/Sec   : 5303.505135
Iterations       : 110000
Compiler version : GCC14.1.0
Compiler flags   : -O2   -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5303.505135 / GCC14.1.0 -O2   -lrt / Heap

```

### 4. CoreMark Benchmark with B Extension (-march=rv64gc_zba_zbb_zbc -mabi=lp64d -O -lrt)

#### 1. Build CoreMark with specific flags

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ make PORT_DIR=linux64 XCFLAGS="-march=rv64gc_zba_zbb_zbc -mabi=lp64d" link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d  -lrt"\" -DITERATIONS=0 -march=rv64gc_zba_zbb_zbc -mabi=lp64d core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

#### 2. Verify the resulting binary is a RISC-V executable

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=bfadfb9c9e64c9217635cda0eb0b12c59a3da3b7, for GNU/Linux 4.15.0, with debug_info, not stripped
```

#### 3. CoreMark score

```bash
«Ruyi venv-gnu-plct-250401» user@milkv:~/coremark-plct-250401$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 19155
Total time (secs): 19.155000
Iterations/Sec   : 5742.625946
Iterations       : 110000
Compiler version : GCC14.1.0
Compiler flags   : -O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d  -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5742.625946 / GCC14.1.0 -O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d  -lrt / Heap
```

### 5. Cross Compile

#### 0. Host Environment

- OS: Ubuntu22.04 LTS x86_64
- Kernel: 6.8.0-52-generic

#### 1. Install Ruyi and Activate Virtual Environment

**Install Ruyi:**

```bash
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
(base) saicogn-rk@saicogn-rk:~$ 

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

**Install gnu-plct toolchain:**

```bash
(base) saicogn-rk@saicogn-rk:~$ ruyi install toolchain/gnu-plct
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250401-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz to /home/saicogn-rk/.cache/ruyi/distfiles/RuyiSDK-20250401-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  159M  100  159M    0     0  10.0M      0  0:00:15  0:00:15 --:--:-- 13.4M
info: extracting RuyiSDK-20250401-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20250401.0
info: package gnu-plct-0.20250401.0 installed to /home/saicogn-rk/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250401.0
```

**Activate Virtual Environment:**

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

(base) saicogn-rk@saicogn-rk:~$ . ./venv-gnu-plct-250401/bin/ruyi-activate 
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ 

«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~$ ruyi-deactivate 
(base) saicogn-rk@saicogn-rk:~$ 
```

**Compiler version check:**

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

**Extract CoreMark package with Ruyi:**

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

**Modify the build configuration to use the RISC-V compiler:**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ grep "gcc" ./linux64/core_portme.mak 
CC = gcc
LD    = gcc
# E.g. generate profile guidance files. Sample PGO generation for gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ grep "gcc" ./linux64/core_portme.mak 
CC = riscv64-plct-linux-gnu-gcc
LD    = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

#### 2. CoreMark BenchMark with Default Flag (-O -lrt)

**Build CoreMark:**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

**Verify the resulting binary is a RISC-V executable:**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=2fa85c4df062f71386df2ffa1f1985a1daaf59be, for GNU/Linux 4.15.0, with debug_info, not stripped
```

**Send to Milk-V Mars:**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark_plct$ scp ./coremark.exe user@192.168.137.50:~
user@192.168.137.50's password: 
coremark.exe                         100%   27KB   1.3MB/s   00:00    
```

**CoreMark score:**

```bash
user@milkv:~$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 20737
Total time (secs): 20.737000
Iterations/Sec   : 5304.528138
Iterations       : 110000
Compiler version : GCC14.1.0
Compiler flags   : -O2   -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5304.528138 / GCC14.1.0 -O2   -lrt / Heap
```

#### 3. CoreMark Benchmark with with B Extenion (-march=rv64gc_zba_zbb_zbc -O2 -lrt)

**Build CoreMark:**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ make PORT_DIR=linux64 XCFLAGS="-march=rv64gc_zba_zbb_zbc -mabi=lp64d" link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d  -lrt"\" -DITERATIONS=0 -march=rv64gc_zba_zbb_zbc -mabi=lp64d core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

**Verify the resulting binary is a RISC-V executable:**

```bash
«Ruyi venv-gnu-plct-250401» (base) saicogn-rk@saicogn-rk:~/coremark-plct-250401$ file coremark.exe 
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=335f7cba65517ad5d88a0b1126bf86086fc53b0f, for GNU/Linux 4.15.0, with debug_info, not stripped
```

**Send to Milk-V Mars:**

```bash
«Ruyi venv-gnu-plct» (base) saicogn-rk@saicogn-rk:~/coremark_plct$ scp ./coremark.exe user@192.168.137.50:~
user@192.168.137.50's password: 
coremark.exe                         100%   27KB   1.3MB/s   00:00    
```

**CoreMark score:**

```bash
user@milkv:~$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 19191
Total time (secs): 19.191000
Iterations/Sec   : 5731.853473
Iterations       : 110000
Compiler version : GCC14.1.0
Compiler flags   : -O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d  -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x33ff
Correct operation validated. See readme.txt for run and reporting rules.
CoreMark 1.0 : 5731.853473 / GCC14.1.0 -O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d  -lrt / Heap

```

### 6. CoreMark Results Comparison

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results with different compilation flags are summarized in the table below:

| Metric                | Native Default | Native with B | Cross Default | Cross with B | Description                                                  |
| --------------------- | -------------- | --------------| ------------- | ------------ |------------------------------------------------------------ |
| **Iterations/Sec**    |  5303.505135   | 5742.625946   | 5304.528138   | 5731.853473  | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 20741          | 19155         | 20737         | 19191        | Total number of clock cycles |
| **Total time (secs)** | 20.741         | 19.155        | 20.737        | 19.191       | Total execution time in seconds |
| **Iterations**        | 110000         | 110000        | 110000        | 110000       | Total number of iterations performed                 |
| **Compiler version**  | GCC14.1.0      |  GCC14.1.0    | GCC14.1.0     | GCC14.1.0    | Compiler used for the test |
| **Compiler flags**    | -O2 -lrt       | -O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d -lrt | -O2 -lrt | -O2 -march=rv64gc_zba_zbb_zbc -mabi=lp64d  -lrt | Compilation flags used |
| **Memory location**   | Heap           | Heap          | Heap          | Heap         | Where data is stored during execution |

These results demonstrate the performance of the StarFive JH7110 (U74) SoC when running CoreMark compiled with the RuyiSDK toolchain with different compilation flags. The B(bit) extension flags showed better performance compared to default flags in this benchmark.

## Test Summary

The following table summarizes the test results for GNU Toolchain on StarFive JH7110 (U74) SoC:

| Test Case | Expected Result | Actual Result | Status |
|-----------|----------------|---------------|--------|
| **Toolchain Installation** | Successfully installed toolchain | Installed to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240401.0` | ✅ PASS |
| **Compiler Verification** | GCC 14.1.0 for RISC-V architecture | GCC 13.1.0 with rv64gc architecture, lp64d ABI | ✅ PASS |
| **Hello World Test** | Successful compilation and execution | Successfully compiled and executed | ✅ PASS |
| **CoreMark Benchmark (Default native)** | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5303.505135 | ✅ PASS |
| **CoreMark Benchmark (Default cross)** | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5304.528138 | ✅ PASS |
| **CoreMark Benchmark (B extension native)** | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5742.625946 | ✅ PASS |
| **CoreMark Benchmark (B extension crosss)** | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5731.853473 | ✅ PASS |

All tests passed successfully, confirming that the PLCT GNU Toolchain works correctly on the StarFive JH7110 (U74) SoC.
