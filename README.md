# LTQMDDs : Quantum Multiple-Valued Decision Diagrams with Linear Transformations

LTQMDDs (Linearly Transformed Quantum Multiple-Valued Decision Diagrams) are a novel canonical representation in quantum computing, integrating linear transformations into QMDDs to obtain more compact forms of quantum functions, with semantics based on the rearrangement of matrix entries and involving level exchange procedures and linear sifting algorithms for optimization.

## 1.ENVIRONMENT:

This repository have been compiled and executed in Ubuntu22.04LTS, 64bit with a machine equipped with an Intel Xeon Gold 6248R 3.00GHz CPU and 128GB memory.



## 2.GETTING THE SOURCES

Open a command prompt in a suitable directory and enter the following command:

```shell
git clone https://github.com/leeotus/mqt-core-ltqmdd-v1.git ./ltqmdd
cd ./ltqmdd
```



## 3.BUILD INSTRUCTIONS

Prerequisites: cmake > 3.10

```cmake
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release
```



## 4.USAGE:

```shell
./build/apps/ltqmddv1 <filename>
```

Take "alu4_201.real" file (from [RevLib](http://revlib.org/)) as an example:

```shell
./build/apps/ltqmddv1 ./<your_path_to_file>/alu4_201.real
```

Currently available files are:

- `Real` (e.g. from [RevLib](http://revlib.org))
- `OpenQASM` (e.g. used by [Qiskit](https://github.com/Qiskit/qiskit))
- `GRCS` Google Random Circuit Sampling Benchmarks (see [GRCS](https://github.com/sboixo/GRCS))
- `TFC` (e.g. from [Reversible Logic Synthesis Benchmarks Page](http://webhome.cs.uvic.ca/~dmaslov/mach-read.html))
- `QC` (e.g. from [Feynman](https://github.com/meamy/feynman))

or, you can just run one file:

```shell
chmod +x ./ltqmdd-test
./ltqmdd-test <filename>
```

to get the results.