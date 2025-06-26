# Sophgo CV1800B GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information

- RuyiSDK on Mil-V Duo with Sophgo CV1800B SoC
- Board OS: Milk-V Duo official BuildRoot image (Releases V1.1.4)
- Host Environment: Fedora Linux 42 (KDE Plasma Desktop Edition) x86_64
- Testing date: June 26, 2025

### Hardware Information

- Milk-V Duo
- Sophgo CV1800B SoC

## Installation

### 1. Install Toolchain

**Command:**

```bash
ruyi install toolchain/gnu-plct
```

**Result:**

```bash
ruixi@fedora:~$ ruyi install toolchain/gnu-plct
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz to /home/ruixi/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  363M  100  363M    0     0  8532k      0  0:00:43  0:00:43 --:--:-- 7850k
info: extracting RuyiSDK-20250615-PLCT-Sources-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20250615.0
info: package gnu-plct-0.20250615.0 installed to /home/ruixi/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250615.0

```

### 2. Create Virtual Environment

**Command:**

```bash
ruyi venv -t toolchain/gnu-plct generic CV1800B_GCC_16_0_0
```

**Result:**

```bash
ruixi@fedora:~/Projects/riscv$ ruyi venv -t toolchain/gnu-plct generic CV1800B_GCC_16_0_0
info: Creating a Ruyi virtual environment at CV1800B_GCC_16_0_0...
info: The virtual environment is now created.

You may activate it by sourcing the appropriate activation script in the
bin directory, and deactivate by invoking `ruyi-deactivate`.

A fresh sysroot/prefix is also provisioned in the virtual environment.
It is available at the following path:

    /home/ruixi/Projects/riscv/CV1800B_GCC_16_0_0/sysroot

The virtual environment also comes with ready-made CMake toolchain file
and Meson cross file. Check the virtual environment root for those;
comments in the files contain usage instructions.
```

**Verification of created environment contents:**

```bash
ruixi@fedora:~/Projects/riscv$ ls CV1800B_GCC_16_0_0 
bin                                     ruyi-cache.v2.toml  sysroot.riscv64-plct-linux-gnu
meson-cross.ini                         ruyi-venv.toml      toolchain.cmake
meson-cross.riscv64-plct-linux-gnu.ini  sysroot             toolchain.riscv64-plct-linux-gnu.cmake

ruixi@fedora:~/Projects/riscv$ ls CV1800B_GCC_16_0_0/bin 
riscv64-plct-linux-gnu-addr2line   riscv64-plct-linux-gnu-gcov-tool             riscv64-plct-linux-gnu-gprofng-display-text
riscv64-plct-linux-gnu-ar          riscv64-plct-linux-gnu-gdb                   riscv64-plct-linux-gnu-gstack
riscv64-plct-linux-gnu-as          riscv64-plct-linux-gnu-gdb-add-index         riscv64-plct-linux-gnu-ld
riscv64-plct-linux-gnu-c++         riscv64-plct-linux-gnu-gfortran              riscv64-plct-linux-gnu-ld.bfd
riscv64-plct-linux-gnu-cc          riscv64-plct-linux-gnu-gp-archive            riscv64-plct-linux-gnu-ldd
riscv64-plct-linux-gnu-c++filt     riscv64-plct-linux-gnu-gp-collect-app        riscv64-plct-linux-gnu-lto-dump
riscv64-plct-linux-gnu-cpp         riscv64-plct-linux-gnu-gp-display-html       riscv64-plct-linux-gnu-nm
riscv64-plct-linux-gnu-elfedit     riscv64-plct-linux-gnu-gp-display-src        riscv64-plct-linux-gnu-objcopy
riscv64-plct-linux-gnu-g++         riscv64-plct-linux-gnu-gp-display-text       riscv64-plct-linux-gnu-objdump
riscv64-plct-linux-gnu-gcc         riscv64-plct-linux-gnu-gprof                 riscv64-plct-linux-gnu-ranlib
riscv64-plct-linux-gnu-gcc-ar      riscv64-plct-linux-gnu-gprofng               riscv64-plct-linux-gnu-readelf
riscv64-plct-linux-gnu-gcc-nm      riscv64-plct-linux-gnu-gprofng-archive       riscv64-plct-linux-gnu-size
riscv64-plct-linux-gnu-gcc-ranlib  riscv64-plct-linux-gnu-gprofng-collect-app   riscv64-plct-linux-gnu-strings
riscv64-plct-linux-gnu-gcov        riscv64-plct-linux-gnu-gprofng-display-html  riscv64-plct-linux-gnu-strip
riscv64-plct-linux-gnu-gcov-dump   riscv64-plct-linux-gnu-gprofng-display-src   ruyi-activate
```

This step created a virtual environment at `$WORK_DIR/CV1800B_GCC_16_0_0/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

### 3. Activate Virtual Environment

**Command:**

```bash
source CV1800B_GCC_16_0_0/bin/ruyi-activate
```

**Result:**
The prompt changed to indicate active environment:

```bash
ruixi@fedora:~/Projects/riscv$ source CV1800B_GCC_16_0_0/bin/ruyi-activate
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv$ 
```

This initialized the environment and provided access to all cross-compilation tools.

To exit virtual environment:

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv$ ruyi-deactivate 
ruixi@fedora:~/Projects/riscv$ 
```

## Tests & Results

### 1. Compiler Version Check

**Command：**

```bash
riscv64-plct-linux-gnu-gcc -v
```

**Result:**

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv$ riscv64-plct-linux-gnu-gcc -v
Using built-in specs.
COLLECT_GCC=/home/ruixi/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250615.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/ruixi/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250615.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/16.0.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=x86_64-build_pc-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250615 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --enable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/riscv64-plct-linux-gnu/buildtools --with-mpfr=/work/riscv64-plct-linux-gnu/buildtools --with-mpc=/work/riscv64-plct-linux-gnu/buildtools --with-isl=/work/riscv64-plct-linux-gnu/buildtools --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
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

**Commands：**

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv$ mkdir CV1800B_GCC_16_0_0/sysroot/root
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv$ cd CV1800B_GCC_16_0_0/sysroot/root
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ vim hello.c
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ cat hello.c 
#include <stdio.h>

int main(void)
{
    printf("Hello World!\r\n");
    return 0;
}
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ riscv64-plct-linux-gnu-gcc hello.c -o hello -static
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ file hello
hello: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (GNU/Linux), statically linked, BuildID[sha1]=28e90e599395aa658b42295449cf00c68848468c, for GNU/Linux 4.15.0, with debug_info, not stripped
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ scp hello root@192.168.42.1:/root
root@192.168.42.1's password: 
sh: /usr/libexec/sftp-server: not found
scp: Connection closed
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ cat hello | ssh root@192.168.42.1 "cat > /root/hello"
root@192.168.42.1's password: 

[root@milkv-duo]~# chmod +x hello
[root@milkv-duo]~# ./hello
Hello World!
```

The program executed successfully and output "Hello World!", confirming basic functionality of the toolchain and proper integration with the system libraries.

### 3. CoreMark Benchmark (-O2 -lrt -static)

#### 1. Clone CoreMark repo

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ git clone https://github.com/eembc/coremark.git
正克隆到 'coremark'...
remote: Enumerating objects: 412, done.
remote: Counting objects: 100% (175/175), done.
remote: Compressing objects: 100% (70/70), done.
remote: Total 412 (delta 136), reused 115 (delta 105), pack-reused 237 (from 2)
接收对象中: 100% (412/412), 542.69 KiB | 541.00 KiB/s, 完成.
处理 delta 中: 100% (234/234), 完成.
```

#### 2. Modify the build configuration to use the RISC-V compiler

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root$ cd coremark/
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root/coremark$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' ./posix/core_portme.mak
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root/coremark$ grep "gcc" ./posix/core_portme.mak
LD              = riscv64-plct-linux-gnu-gcc
# E.g. generate profile guidance files. Sample PGO generation for riscv64-plct-linux-gnu-gcc enabled with PGO=1
  PGO_STAGE=build_pgo_gcc
.PHONY: build_pgo_gcc
build_pgo_gcc:
```

#### 3. CoreMark BenchMark with Default Flag (-O -lrt -static)

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root/coremark$ make PORT_DIR=posix XCFLAGS="-static" CC=riscv64-plct-linux-gnu-gcc link
riscv64-plct-linux-gnu-gcc -O2 -Iposix -Iposix -I. -DFLAGS_STR=\""-O2 -static  -lrt"\" -DITERATIONS=0 -static core_list_join.c core_main.c core_matrix.c core_state.c core_util.c posix/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile
```

#### 4. Verify the resulting binary is a RISC-V executable

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root/coremark$ file coremark.exe
coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (GNU/Linux), statically linked, BuildID[sha1]=c691e085cfa189cbcf96e749e5888ded6d0ea0ad, for GNU/Linux 4.15.0, with debug_info, not stripped
```

**Send to Milk-V Duo:**

```bash
«Ruyi CV1800B_GCC_16_0_0» ruixi@fedora:~/Projects/riscv/CV1800B_GCC_16_0_0/sysroot/root/coremark$ cat coremark.exe | ssh root@192.168.42.1 "cat > /root/coremark.exe"
root@192.168.42.1's password: 
```

#### 5. CoreMark score

```bash
[root@milkv-duo]~# chmod +x coremark.exe 
[root@milkv-duo]~# ./coremark.exe 
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 13799
Total time (secs): 13.799000
Iterations/Sec   : 2174.070585
Iterations       : 30000
Compiler version : GCC16.0.0 20250512 (experimental)
Compiler flags   : -O2 -static  -lrt
Memory location  : Please put data memory location here
                        (e.g. code in flash, data on heap etc)
seedcrc          : 0xe9f5
[0]crclist       : 0xe714
[0]crcmatrix     : 0x1fd7
[0]crcstate      : 0x8e3a
[0]crcfinal      : 0x5275
Correct operation validated. See README.md for run and reporting rules.
CoreMark 1.0 : 2174.070585 / GCC16.0.0 20250512 (experimental) -O2 -static  -lrt / Heap
```

#### 6. CoreMark Results

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value                             | Description                                                  |
| --------------------- | --------------------------------- | ------------------------------------------------------------ |
| **Iterations/Sec**    | 2174.070585                       | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 13799                             | Total number of clock cycles                                 |
| **Total time (secs)** | 13.799000                         | Total execution time in seconds                              |
| **Iterations**        | 30000                             | Total number of iterations performed                         |
| **Compiler version**  | GCC16.0.0 20250512 (experimental) | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt -static                  | Compilation flags used                                       |
| **Memory location**   | Heap                              | Where data is stored during execution                        |

These results demonstrate good performance of the Sophgo CV1800B SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Sophgo CV1800B SoC:

| Test Case                  | Expected Result                        | Actual Result                                                | Status |
| -------------------------- | -------------------------------------- | ------------------------------------------------------------ | ------ |
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `/home/ruixi/.local/share/ruyi/binaries/x86_64/gnu-plct-0.20250615.0` | ✅ PASS |
| **Compiler Verification**  | GCC 16.0.0 for RISC-V architecture     | GCC 16.0.0 with rv64gc architecture, lp64d ABI               | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                           | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 2174.070585 | ✅ PASS |

All tests passed successfully, confirming that the GNU Upstream Toolchain works correctly on the Sophgo CV1800B SoC.
