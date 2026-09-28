# Experimental Compiler

I built this C++ learning project to understand what happens between a small source program and generated assembly. It parses a language with integer variables and functions, prints the syntax tree, and writes an experimental assembly output to `code.S`.

## Status

**Learning prototype with incomplete language and code generation support.**

The implemented backend emits x86-64 AT&T assembly using the Microsoft Windows calling convention. ARM and general cross-platform executable generation are not implemented. The code contains unfinished cases, so generated output should be inspected rather than assumed correct.

## Features and Plans

- [X] Lexical Analysis: Tokenizing the input source code.
- [X] Syntax Analysis: Parsing the tokens to build an Abstract Syntax Tree (AST).
- [ ] Semantic Analysis: Checking for semantic correctness and building a symbol table.
- [ ] Intermediate Code Generation: Converting the AST into intermediate code.
- [ ] Optimization: Implementing basic optimizations on the intermediate code.
- [X] Experimental code generation: emitting Windows x86-64 AT&T assembly directly from parsed structures.

## Getting Started

### Prerequisites

- A C++14-capable compiler and CMake 3.14 or newer.
- Windows/MinGW assembler and linker tools if you want to investigate the generated assembly.

### Building the Compiler

1. Clone this repository to your local machine:

   ```bash
   git clone https://github.com/eldarmuk/Experimental-Compiler.git
   cd Experimental-Compiler
   ```

2. Build the project using CMake:

    ```bash
    cmake -S . -B build -DCMAKE_CXX_STANDARD=14
    cmake --build build
    ```

   The explicit standard flag avoids relying on the incomplete standard-setting line in the current CMake file. The executable target is named `func`; its location varies by CMake generator (`build/func`, `build/func.exe` or `build/Debug/func.exe`).

3. Run it with the supplied example, adjusting the executable path for your build:

    ```sh
    ./build/func example.txt
    ```

   `example.txt` includes declarations such as `a : integer = 69`, reassignment with `a := 420`, a function declaration and `foo(20, 34)`. The driver prints the parsed tree and writes `code.S` in the working directory. It returns 1 for a parsing error and 2 for a code-generation error.

   See [`src/parser.cpp`](src/parser.cpp), [`src/environment.cpp`](src/environment.cpp) and [`src/codegen.cpp`](src/codegen.cpp) for the implementation.

4. To investigate the generated x86-64 assembly:

    On Windows under MinGW:

    ```bash
    as code.S -o code.o
    ld code.o -o code.exe
    ```

   These are the original MinGW-oriented assembly steps, not a guarantee that every generated program links or runs correctly. There is no automated test suite in this repository, and the compiler was not rebuilt during this documentation update.

### Known boundaries

Function argument generation assumes integer values in several places; stack arguments and other expression cases contain TODOs. The example's proposed lambda syntax is also unimplemented. A complete semantic-analysis pass, intermediate representation and optimizer remain outside the demonstrated scope.

### Contributing

Contributions are welcome! If you find any bugs, have suggestions, or want to add new features, feel free to open an issue or submit a pull request.

### License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### Resources

This project was inspired by the following repository: [Intercept](https://github.com/LensPlaysGames/Intercept.git). I am writing this compiler in C++ with insights and ideas drawn from that repository.
