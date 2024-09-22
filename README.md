# CMake build system for GNU Bison

GNU Bison is a parser generator. This repository includes the CMake-based build
system files to build GNU Bison from scratch using CMake.

## Usage

```sh
cmake -S . -B build --install-prefix=$(pwd)/install
cmake --build build -j
cmake --install build
./install/bin/bison --version
```
