# Allwinner D1 GNU Toolchain (gnu-plct) Test Report

## Environment

### System Information
- RuyiSDK on Allwinner D1 boards
    - AWOL Nezha in this test report, should be the same on LicheeRV Dock
- OS: Ubuntu 24.04.2 LTS
- ruyi package manager version: ruyi 0.36.0
    - 0.37.0 failed to create venv so not using it
- Testing date: July 9, 2025

### Hardware Information
- Sipeed LicheeRV Dock / AWOL Nezha (D1-H)
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
info: downloading https://mirror.iscas.ac.cn/ruyisdk/dist/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz to /home/ubuntu/.cache/ruyi/distfiles/RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   142  100   142    0     0    101      0  0:00:01  0:00:01 --:--:--   101
100  359M  100  359M    0     0  4097k      0  0:01:29  0:01:29 --:--:-- 3971k
info: extracting RuyiSDK-20250615-PLCT-Sources-HOST-riscv64-linux-gnu-riscv64-plct-linux-gnu.tar.xz for package gnu-plct-0.20250615.0
info: package gnu-plct-0.20250615.0 installed to /home/ubuntu/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0
```

This command downloaded the toolchain package (~359MB) from ISCAS mirror and installed it to `$HOME$/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0`.

### 2. Create Virtual Environment

Failed to create venv.

**Command:**
```bash
ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
```

**Result on 0.36.0**
```bash
ubuntu@ubuntu:~$ time ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
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

**Result on 0.37.0:**
```bash
ubuntu@ubuntu:~$ ruyi venv -t toolchain/gnu-plct generic venv-gnu-plct
info: Creating a Ruyi virtual environment at venv-gnu-plct...
Traceback (most recent call last):
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/__main__.py", line 98, in <module>
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/__main__.py", line 94, in entrypoint
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/ruyi/cli/main.py", line 109, in main
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/ruyi/mux/venv/venv_cli.py", line 81, in main
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/ruyi/mux/venv/maker.py", line 310, in do_make_venv
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/ruyi/mux/venv/maker.py", line 419, in provision
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/ruyi/mux/venv/maker.py", line 377, in render_and_write
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/ruyi/utils/templating.py", line 33, in render_template_str
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/jinja2/environment.py", line 1016, in get_template
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/jinja2/environment.py", line 975, in _load_template
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/jinja2/loaders.py", line 126, in load
  File "/home/ubuntu/.cache/ruyi/progcache/0.37.0/linux-riscv64/ruyi/utils/templating.py", line 20, in get_source
jinja2.exceptions.TemplateNotFound: ruyi-venv.toml
```

**Verification of created environment contents:**

```bash
ubuntu@ubuntu:~$ ls ~/venv-gnu-plct/
bin              meson-cross.riscv64-plct-linux-gnu.ini  ruyi-venv.toml  sysroot.riscv64-plct-linux-gnu  toolchain.riscv64-plct-linux-gnu.cmake
meson-cross.ini  ruyi-cache.v2.toml                      sysroot         toolchain.cmake
ubuntu@ubuntu:~$ ls ~/venv-gnu-plct/bin
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
ubuntu@ubuntu:~$
```

This step created a virtual environment at `$HOME/venv-gnu-plct/` with all necessary configuration files, including Meson cross-compilation files, CMake toolchain files, a ready-to-use sysroot, and binary tools with the prefix `riscv64-plct-linux-gnu-`.

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
COLLECT_GCC=/home/ubuntu/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0/bin/riscv64-plct-linux-gnu-gcc
COLLECT_LTO_WRAPPER=/home/ubuntu/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0/bin/../libexec/gcc/riscv64-plct-linux-gnu/16.0.0/lto-wrapper
Target: riscv64-plct-linux-gnu
Configured with: /work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/src/gcc/configure --build=x86_64-build_pc-linux-gnu --host=riscv64-host_unknown-linux-gnu --target=riscv64-plct-linux-gnu --prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --exec_prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu --with-sysroot=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-languages=c,c++,fortran,objc,obj-c++ --with-arch=rv64gc --with-abi=lp64d --with-pkgversion='RuyiSDK 20250615 PLCT-Sources' --with-bugurl=https://github.com/ruyisdk/ruyisdk/issues --enable-__cxa_atexit --disable-libmudflap --enable-libgomp --disable-libquadmath --disable-libquadmath-support --disable-libmpx --with-gmp=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpfr=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-mpc=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --with-isl=/work/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/buildtools/complibs-host --enable-lto --enable-threads=posix --enable-target-optspace --enable-linker-build-id --with-linker-hash-style=gnu --enable-plugin --disable-nls --enable-multiarch --with-local-prefix=/opt/ruyi/HOST-riscv64-linux-gnu/riscv64-plct-linux-gnu/riscv64-plct-linux-gnu/sysroot --enable-long-long
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 16.0.0 20250512 (experimental) (RuyiSDK 20250615 PLCT-Sources) 
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$
```

The output confirmed a successful installation showing:
- GCC version: 16.0.0 20250512 (experimental) (RuyiSDK 20250615 PLCT-Sources) 
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
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~$ mkdir coremark; pushd coremark
~/coremark ~
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~/coremark$ ruyi extract coremark
info: extracting coremark-1.01.tar.gz for package coremark-1.0.1
info: package coremark-1.0.1 extracted to current working directory
```

2. Modify the build configuration to use the RISC-V compiler:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~/coremark$ sed -i 's/\bgcc\b/riscv64-plct-linux-gnu-gcc/g' linux64/core_portme.mak
```

3. Build CoreMark:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~/coremark$ make PORT_DIR=linux64 link
riscv64-plct-linux-gnu-gcc -O2 -Ilinux64 -I. -DFLAGS_STR=\""-O2   -lrt"\" -DITERATIONS=0  core_list_join.c core_main.c core_matrix.c core_state.c core_util.c linux64/core_portme.c -o ./coremark.exe -lrt
Link performed along with compile

```

4. Verify the resulting binary is a RISC-V executable:
```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~/coremark$ ls -al
total 160
drwxrwxr-x 8 ubuntu ubuntu  4096 Jul  9 10:46 .
drwxr-x--- 9 ubuntu ubuntu  4096 Jul  9 10:44 ..
-rw-rw-r-- 1 ubuntu ubuntu  9416 May 23  2018 LICENSE.md
-rw-rw-r-- 1 ubuntu ubuntu  3678 May 23  2018 Makefile
-rw-rw-r-- 1 ubuntu ubuntu 18799 May 23  2018 README.md
drwxrwxr-x 2 ubuntu ubuntu  4096 May 23  2018 barebones
-rw-rw-r-- 1 ubuntu ubuntu 14651 May 23  2018 core_list_join.c
-rw-rw-r-- 1 ubuntu ubuntu 12503 May 23  2018 core_main.c
-rw-rw-r-- 1 ubuntu ubuntu  8097 May 23  2018 core_matrix.c
-rw-rw-r-- 1 ubuntu ubuntu  7186 May 23  2018 core_state.c
-rw-rw-r-- 1 ubuntu ubuntu  5171 May 23  2018 core_util.c
-rwxrwxr-x 1 ubuntu ubuntu 27224 Jul  9 10:46 coremark.exe
-rw-rw-r-- 1 ubuntu ubuntu  4373 May 23  2018 coremark.h
drwxrwxr-x 2 ubuntu ubuntu  4096 May 23  2018 cygwin
drwxrwxr-x 3 ubuntu ubuntu  4096 May 23  2018 docs
drwxrwxr-x 2 ubuntu ubuntu  4096 May 23  2018 linux
drwxrwxr-x 2 ubuntu ubuntu  4096 Jul  9 10:45 linux64
drwxrwxr-x 2 ubuntu ubuntu  4096 May 23  2018 simple
```

5. CoreMark score

```bash
«Ruyi venv-gnu-plct» ubuntu@ubuntu:~/coremark$ ./coremark.exe
2K performance run parameters for coremark.
CoreMark Size    : 666
Total ticks      : 15544
Total time (secs): 15.544000
Iterations/Sec   : 2573.340196
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
CoreMark 1.0 : 2573.340196 / GCC16.0.0 20250512 (experimental) -O2   -lrt / Heap
```

**CoreMark Results:**

CoreMark is a benchmark used to evaluate embedded processor performance. A higher score indicates better processor performance. The results are summarized in the table below:

| Metric                | Value       | Description                                                  |
|-----------------------|-------------|--------------------------------------------------------------|
| **Iterations/Sec**    | 2573.340196 | Number of iterations completed per second (higher is better) |
| **Total ticks**       | 15544       | Total number of clock cycles                                 |
| **Total time (secs)** | 15.544000   | Total execution time in seconds                              |
| **Iterations**        | 40000       | Total number of iterations performed                         |
| **Compiler version**  | GCC16.0.0   | Compiler used for the test                                   |
| **Compiler flags**    | -O2 -lrt    | Compilation flags used                                       |
| **Memory location**   | Heap        | Where data is stored during execution                        |

These results demonstrate good performance of the Allwinner D1 SoC when running CoreMark compiled with the RuyiSDK toolchain.

## Test Summary

The following table summarizes the test results for GNU Toolchain on Allwinner D1:

| Test Case                  | Expected Result                        | Actual Result                                                                 | Status  |
|----------------------------|----------------------------------------|-------------------------------------------------------------------------------|---------|
| **Toolchain Installation** | Successfully installed toolchain       | Installed to `$HOME/.local/share/ruyi/binaries/riscv64/gnu-plct-0.20250615.0` | ✅ PASS |
| **Compiler Verification**  | GCC 16.0.0 for RISC-V architecture     | GCC 16.0.0 with rv64gc architecture, lp64d ABI                                | ✅ PASS |
| **Hello World Test**       | Successful compilation and execution   | Successfully compiled and executed                                            | ✅ PASS |
| **CoreMark Benchmark**     | Successfully compile and run benchmark | Successfully compiled and completed benchmark with score of 2573.340196       | ✅ PASS |

All tests passed successfully, confirming that the GNU Toolchain works correctly on the Allwinner D1 SoC.

