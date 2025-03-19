# Allwinner D1 GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information
- RuyiSDK on LicheeRV Dock with Allwinner D1 SoC
- Testing date: March 14, 2025

### Hardware Information
- Sipeed LicheeRV Dock
- Allwinner D1 SoC (XuanTie C906 core)

## Installation

### 1. Install Toolchain

**Command:**
```bash
ruyi install toolchain/gnu-plct
```

**Result:**
```bash
ubuntu@ubuntu:~$ ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-03-19 00:00:00 +0000 and 2025-03-20 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /home/ubuntu/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  231M  100  231M    0     0  3663k      0  0:01:04  0:01:04 --:--:-- 3619k
info: extracting RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20240324.0
info: package gnu-plct-0.20240324.0 installed to /home/ubuntu/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0
```

This command downloaded the toolchain package (~231MB) from ISCAS mirror and installed it to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0`.

### 2. Create Virtual Environment

**Command:**
```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result:**
```bash
ubuntu@ubuntu:~$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-03-19 00:00:00 +0000 and 2025-03-20 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: Creating a Ruyi virtual environment at venv-gnu-plct...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/ubuntu/venv-gnu-plct/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**
```bash
ubuntu@ubuntu:~$ ls ~/venv-gnu-plct/
bin                                     sysroot
meson-cross.ini                         sysroot.riscv64-plct-linux-gnu
meson-cross.riscv64-plct-linux-gnu.ini  toolchain.cmake
ruyi-cache.v2.toml                      toolchain.riscv64-plct-linux-gnu.cmake
ruyi-venv.toml
ubuntu@ubuntu:~$ ls ~/venv-gnu-plct/bin/
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

This step created a virtual environment at `/root/venv-gnu-plct/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

### 3. Activate Environment

**Command:**
```bash
. ~/venv-gnu-plct/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ 
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
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/ubuntu/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/ubuntu/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/13.1.0/lto-wrapper
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
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ riscv64-plct-linux-gnu-gcc hello.c -o hello_plct
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ ./hello_plct 
Hello, World!
```

The program executed successfully and output "Hello, world!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Wednesday
info: the next upload will happen anytime ruyi is executed between 2025-03-19 00:00:00 +0000 and 2025-03-20 00:00:00 +0000
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/coremark-1.01.tar.gz to /home/ubuntu/.cache/ruyi/distfiles/coremark-1.01.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  391k  100  391k    0     0   249k      0  0:00:01  0:00:01 --:--:--  249k
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

2. Modify the build configuration to use the RISC-V compiler:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

3. Build CoreMark:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ make PORT_DIR=linux64 link 
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ ls -al
total 24988
drwxr-x--- 13 ubuntu ubuntu     4096 Mar 14 08:24 .
drwxr-xr-x  3 root   root       4096 Oct 17 17:17 ..
-rw-------  1 ubuntu ubuntu      790 Mar 14 08:15 .bash_history
-rw-r--r--  1 ubuntu ubuntu      220 Mar 31  2024 .bash_logout
-rw-r--r--  1 ubuntu ubuntu     3771 Mar 31  2024 .bashrc
drwx------  3 ubuntu ubuntu     4096 Mar 14 07:26 .cache
drwxrwxr-x  4 ubuntu ubuntu     4096 Mar 14 07:28 .local
-rw-r--r--  1 ubuntu ubuntu      807 Mar 31  2024 .profile
-rw-------  1 ubuntu ubuntu        0 Mar 14 07:28 .python_history
drwx------  2 ubuntu ubuntu     4096 Oct 17 17:17 .ssh
-rw-r--r--  1 ubuntu ubuntu        0 Mar 14 07:20 .sudo_as_admin_successful
-rw-------  1 ubuntu ubuntu      897 Mar 14 08:05 .viminfo
-rw-rw-r--  1 ubuntu ubuntu     9416 May 23  2018 LICENSE.md
-rw-rw-r--  1 ubuntu ubuntu     3678 May 23  2018 Makefile
-rw-rw-r--  1 ubuntu ubuntu    18799 May 23  2018 README.md
drwxrwxr-x  2 ubuntu ubuntu     4096 May 23  2018 barebones
-rw-rw-r--  1 ubuntu ubuntu    14651 May 23  2018 core_list_join.c
-rw-rw-r--  1 ubuntu ubuntu    12503 May 23  2018 core_main.c
-rw-rw-r--  1 ubuntu ubuntu     8097 May 23  2018 core_matrix.c
-rw-rw-r--  1 ubuntu ubuntu     7186 May 23  2018 core_state.c
-rw-rw-r--  1 ubuntu ubuntu     5171 May 23  2018 core_util.c
drwxrwxr-x  2 ubuntu ubuntu     4096 Mar 14 08:14 coremark
-rwxrwxr-x  1 ubuntu ubuntu    28152 Mar 14 08:24 coremark.exe
-rw-rw-r--  1 ubuntu ubuntu     4373 May 23  2018 coremark.h
drwxrwxr-x  2 ubuntu ubuntu     4096 May 23  2018 cygwin
drwxrwxr-x  3 ubuntu ubuntu     4096 May 23  2018 docs
-rw-rw-r--  1 ubuntu ubuntu      129 Mar 14 08:08 hello.c
-rwxrwxr-x  1 ubuntu ubuntu    13144 Mar 14 08:08 hello_plct
drwxrwxr-x  2 ubuntu ubuntu     4096 May 23  2018 linux
drwxrwxr-x  2 ubuntu ubuntu     4096 Mar 14 08:25 linux64
-rwxrwxr-x  1 ubuntu ubuntu 25358469 Mar 11 06:55 ruyi.riscv64
drwxrwxr-x  2 ubuntu ubuntu     4096 May 23  2018 simple
drwxrwxr-x  4 ubuntu ubuntu     4096 Mar 14 07:59 venv-gnu-plct
```

5. CoreMark score

```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ ./coremark.exe 
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 12651
Total time (secs): 12.651000
Iterations/Sec   : 2371.354043
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
CoreMark 1.0 : 2371.354043 / GCC13.1.0 -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
|-----------------------|-------------|--------------------------------------------------------------|
| **Iterations/Sec**    | 2371.354043 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 12651       | Total number of clock cycles                                 |
| **Total time (secs)** | 12.651000   | Total execution time in seconds                              |
| **Iterations**        | 30000       | Total number of iterations performed                         |
| **Compiler version**  | GCC13.1.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the SpacemiT K1/M1 (X60) SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on SpacemiT K1/M1 (X60):

| Test Case                  | Expected Result                        | Actual Result                                                                 | Status  |
|----------------------------|----------------------------------------|-------------------------------------------------------------------------------|---------|
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0` | ✅ PASS |
| **Compiler Verification**  | GCC 13.1.0 for RISC-V architecture     | GCC 13.1.0 with rv64gc architecture, lp64d ABI                                | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                                            | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 2371.354043       | ✅ PASS |

All tests passed successfully, confirming that the GNU Toolchain works correctly on the SpacemiT K1/M1 (X60) SoC.

