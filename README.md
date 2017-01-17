
## At a glance ##
A step-by-step tutorial for building an out-of-source LLVM pass based on Adrian Sampson's "LLVM for Grad Students"

## Setup ##

LLVM is an umbrella project for building compilers
and code transformation tools. We consider in this tutorial:
- Building LLVM from source
- Building a trivial out-of-source LLVM pass.

We will be installing LLVM v`3.9.1` which is the latest as of this writing.
We assume that you have a working compiler toolchain (GCC or LLVM) and that CMake is installed (minimum version 3.4.3).


## Compiling LLVM ##
Compiling LLVM from source is mandatory if you are developing an in-source pass (within LLVM source tree).
It can also be convenient even in the case of developing out-of-source passes as it gives you full control over the compilation options.

1.  Download LLVM [source](http://llvm.org/releases/)
and unpack it in a directory of your choice which will refer to as `$LLVM_SRC`

2. Create a separate build directory
    ```bash
    $ mkdir llvm-build
    $ cd llvm-build
    ```
3. Instruct CMake to detect and configure your build environment:

    ```bash
    $ cmake $LLVM_SRC
    ```
4. Now start the actual compilation within your build directory

    ```bash
    $ cmake --build .
    ```
    The `--build` option is a portable why to tell cmake to invoke the underlying
    build tool (make, ninja, xcodebuild, msbuild, etc.)

5. Building require a while. After that you can install LLVM in its default directory
    which is `/usr/local`
    ```bash
    $ cmake --build . --target install
    ```
    Alternatively, it's possible to set a different install directory (`$LLVM_HOME`)
    ```bash
    $ cmake -DCMAKE_INSTALL_PREFIX=$LLVM_HOME -P cmake_install.cmake
    ```
    Note that `$LLVM_HOME` must __not__ contain `~` (tilde) to refer to your home directory as it won't be expanded. Use absolute paths instead.

## Building a trivial LLVM pass ##

To build the pass found in `skeleton` folder:
```bash
$ cd llvm-pass-tutorial
$ mkdir build
$ cd build
$ LLVM_DIR="$LLVM_HOME/share/llvm/cmake" cmake ..
$ make
```
cmake needs to find its LLVM configurations which we provide using
`$LLVM_DIR`. Alternatively, you can use system-wide cmake configurations
at `/usr/share/llvm-3.8/cmake/` if available.


The easiest way to run the skeleton pass is using Clang which is the compiler
front-end of the LLVM framework:
```bash
$ clang-3.9 -Xclang -load -Xclang build/skeleton/libSkeletonPass.* something.c$
```
Note that Clang needs to be installed separately.

### Further resources
This tutorial is based on the following resources

- Adrian Sampson's blog entry "LLVM for Grad Students" ([link](http://adriansampson.net/blog/llvm.html))
- LLVM documentation: Writing an LLVM pass ([link](http://llvm.org/docs/WritingAnLLVMPass.html))
- LLVM documentation: Building LLVM with CMake ([link](http://llvm.org/docs/CMake.html#cmake-out-of-source-pass))
