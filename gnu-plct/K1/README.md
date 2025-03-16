# SpacemiT K1/M1 (X60) GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information
- RuyiSDK on Banana Pi BPI-F3 with SpacemiT K1/M1 (X60) SoC
- Testing date: March 14, 2025

### Hardware Information
- Banana Pi BPI-F3 board
- SpacemiT K1/M1 SoC (RISC-V SpacemiT X60 core)

## Installation

### 1. Install Toolchain

**Command:**
```bash
ruyi install toolchain/gnu-plct
```

**Result:**
```bash
root@k1:~# ruyi install toolchain/gnu-plct
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: usage information has already been uploaded today at 2025-03-14 03:09:27 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /root/.cache/ruyi/distfiles/RuyiSDK-20240324-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  231M  100  231M    0     0  35.8M      0  0:00:06  0:00:06 --:--:-- 46.2M
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
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: usage information has already been uploaded today at 2025-03-14 03:09:27 +0800
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
```

**Verification of created environment contents:**
```bash
root@k1:~# ls ~/venv-gnu-plct/
bin                                     ruyi-cache.v2.toml  sysroot.riscv64-plct-linux-gnu
meson-cross.ini                         ruyi-venv.toml      toolchain.cmake
meson-cross.riscv64-plct-linux-gnu.ini  sysroot             toolchain.riscv64-plct-linux-gnu.cmake
root@k1:~# ls ~/venv-gnu-plct/bin/
riscv64-plct-linux-gnu-addr2line  riscv64-plct-linux-gnu-gcc-nm         riscv64-plct-linux-gnu-ldd
riscv64-plct-linux-gnu-ar         riscv64-plct-linux-gnu-gcc-ranlib     riscv64-plct-linux-gnu-lto-dump
riscv64-plct-linux-gnu-as         riscv64-plct-linux-gnu-gcov           riscv64-plct-linux-gnu-nm
riscv64-plct-linux-gnu-c++        riscv64-plct-linux-gnu-gcov-dump      riscv64-plct-linux-gnu-objcopy
riscv64-plct-linux-gnu-cc         riscv64-plct-linux-gnu-gcov-tool      riscv64-plct-linux-gnu-objdump
riscv64-plct-linux-gnu-c++filt    riscv64-plct-linux-gnu-gdb            riscv64-plct-linux-gnu-ranlib
riscv64-plct-linux-gnu-cpp        riscv64-plct-linux-gnu-gdb-add-index  riscv64-plct-linux-gnu-readelf
riscv64-plct-linux-gnu-elfedit    riscv64-plct-linux-gnu-gfortran       riscv64-plct-linux-gnu-size
riscv64-plct-linux-gnu-g++        riscv64-plct-linux-gnu-gprof          riscv64-plct-linux-gnu-strings
riscv64-plct-linux-gnu-gcc        riscv64-plct-linux-gnu-ld             riscv64-plct-linux-gnu-strip
riscv64-plct-linux-gnu-gcc-ar     riscv64-plct-linux-gnu-ld.bfd         ruyi-activate
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
«Ruyi venv-gnu-plct» root@k1:~# 
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
Using built-in specs.
COLLECT_GCC=/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/13.1.0/lto-wrapper
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
«Ruyi venv-gnu-plct» root@k1:~# riscv64-plct-linux-gnu-gcc hello.c -o hello_plct
«Ruyi venv-gnu-plct» root@k1:~# ./hello_plct 
Hello, world!
```

The program executed successfully and output "Hello, world!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark

**Commands and Results:**

1. Extract the CoreMark package with Ruyi:
```bash
«Ruyi venv-gnu-plct» root@k1:~/coremark# ruyi extract coremark
warn: this ruyi installation has telemetry mode set to on, and will upload non-tracking usage information to RuyiSDK-managed servers every Thursday
info: usage information has already been uploaded today at 2025-03-14 03:09:27 +0800
info: in order to hide this banner:
info: - opt out with ruyi telemetry optout
info: - or give consent with ruyi telemetry consent
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

2. Modify the build configuration to use the RISC-V compiler:
```bash
«Ruyi venv-gnu-plct» root@k1:~/coremark# sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

3. Build CoreMark:
```bash
«Ruyi venv-gnu-plct» root@k1:~/coremark# make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

4. Verify the resulting binary is a RISC-V executable:
```bash
«Ruyi venv-gnu-upstream» root@k1:~/coremark# ls -al
总计 168
drwxr-xr-x 10 root root  4096  3月14日 04:28 .
drwx------ 11 root root  4096  3月14日 04:26 ..
drwxrwxr-x  2 root root  4096 2018年 5月23日 barebones
-rw-rw-r--  1 root root 14651 2018年 5月23日 core_list_join.c
-rw-rw-r--  1 root root 12503 2018年 5月23日 core_main.c
-rwxr-xr-x  1 root root 28152  3月14日 04:28 coremark.exe
-rw-rw-r--  1 root root  4373 2018年 5月23日 coremark.h
-rw-rw-r--  1 root root  8097 2018年 5月23日 core_matrix.c
-rw-rw-r--  1 root root  7186 2018年 5月23日 core_state.c
-rw-rw-r--  1 root root  5171 2018年 5月23日 core_util.c
drwxrwxr-x  2 root root  4096 2018年 5月23日 cygwin
drwxrwxr-x  3 root root  4096 2018年 5月23日 docs
drwxr-xr-x  8 root root  4096  3月14日 03:41 .git
drwxr-xr-x  3 root root  4096  3月14日 03:41 .github
-rw-rw-r--  1 root root  9416 2018年 5月23日 LICENSE.md
drwxrwxr-x  2 root root  4096 2018年 5月23日 linux
drwxrwxr-x  2 root root  4096  3月14日 04:27 linux64
-rw-rw-r--  1 root root  3678 2018年 5月23日 Makefile
-rw-rw-r--  1 root root 18799 2018年 5月23日 README.md
drwxrwxr-x  2 root root  4096 2018年 5月23日 simple
«Ruyi venv-gnu-plct» root@k1:~/coremark# file coremark.exe 
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=2ec45ce18cb640e991e7f27da56169eaa1652368, for GNU/Linux 4.15.0, with debug_info, not stripped
```

5. CoreMark score

```bash
«Ruyi venv-gnu-plct» root@k1:~/coremark# ./coremark.exe 
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 19365
Total time (secs): 19.365000
Iterations/Sec   : 5680.351149
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
CoreMark 1.0 : 5680.351149 / GCC13.1.0 -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric | Value | Description |
|--------|-------|-------------|
| **Iterations/Sec** | 5680.351149 | Number of iterations completed per second (higher is better) |
| **Total ticks** | 19365 | Total number of clock cycles |
| **Total time (secs)** | 19.365 | Total execution time in seconds |
| **Iterations** | 110000 | Total number of iterations performed |
| **Compiler version** | GCC13.1.0 | Compiler used for the test |
| **Compiler flags** | -O2 -lrt | Compilation flags used |
| **Memory location** | Heap | Where data is stored during execution |

These results demonstrate good performance of the SpacemiT K1/M1 (X60) SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on SpacemiT K1/M1 (X60):

| Test Case | Expected Result | Actual Result | Status |
|-----------|----------------|---------------|--------|
| **Toolchain Installation** | Successfully installed toolchain | Installed to `/root/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20240324.0` | ✅ PASS |
| **Compiler Verification** | GCC 13.1.0 for RISC-V architecture | GCC 13.1.0 with rv64gc architecture, lp64d ABI | ✅ PASS |
| **Hello World Test** | Successful compilation and execution | Successfully compiled and executed | ✅ PASS |
| **CoreMark Benchmark** | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 5680.351149 | ✅ PASS |

All tests passed successfully, confirming that the GNU Toolchain works correctly on the SpacemiT K1/M1 (X60) SoC.

## Logs & Recording

[![asciicast](https://asciinema.org/a/0jf1EVr57UDwpRLafByi9bdv5.svg)](https://asciinema.org/a/0jf1EVr57UDwpRLafByi9bdv5)