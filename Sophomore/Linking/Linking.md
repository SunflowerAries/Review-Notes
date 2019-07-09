# <center>Linking</center>

[TOC]

## Time to link

- Compile time when the source code is translated into machine code
- Load time when the program is loaded into memory by the loader
- Run time by application programs

## Static linking

- Symbol resolution: Object files define and reference symbols corresponding to a function, a global variable or a static variable.
- Relocation: Compilers and assemblers generate code and data sections that start at address 0. The linker relocates these sections by associating a memory location with each symbol definition.

## Object files

- Relocatable object file: Contains binary code and data in a form that can be combined with other relocatable object files at compile time to create an executable object file.
- Executable object file: Contains binary code and data in a form that can be copied directly into memory and executed.
- Shared object file: A special type of relocatable object file that can be loaded into memory and linked dynamically, at either load time or run time.

## Relocatable object files

- .text: The machine code of compiled program
- .rodata: Read-only data
- .data: Initialized global and static C variables.
- .bss: Uninitialized global and static C variables, along with any global or static variables that are initialized to zero.
- .symtab: A symbol table with information about functions and global variables that are defined and referenced in the program.
- .rel.text: A list of locations in the .text section that will need to be modified when the linker combines this object file with others. Any instructions that calls an external function or references a global variable will need to be modified.
- .rel.data: Relocation information for any global variables that are referenced or defined by the module. Any initialized global variable whose initial value is the address of a global variable or externally defined function will need to be modified.
- .debug: A debugging symbol table with entries for local variables and typedefs defined in the program, global variables defined and referenced in the program, and the original C source file. It is only present if the compiler driver is invoked with the -g option.
- .line: A mapping between line numbers in the original C source program and machine code instructions in the .text section. It is only present if the compiler driver is invoked with the -g option.
- .strtab: A string table for the symbol tables in the .symtab and .debug sections and for the section names in the section headers. A string table is a sequence of null-terminated character strings.

## Symbols and symbol tables

- Define: Global symbols that are defined by module m and that can be referenced by other modules. Global linker symbols correspond to nonstatic C functions and global variables.
- Use external: Global symbols that are referenced by module m but defined by some other module. Such symbols are called externals and correspond to nonstatic C functions and global variables that are defined in other modules.
- Local: Local symbols that are defined and referenced exclusively by module m. These correspond to static C functions and global variables that are defined with the static attribute. These symbols are visible anywhere within module m, but cannot be referenced by other modules.

### Hint

- Local linker symbols are not the same as local program variables which are managed at run time on the stack.
- Symbol tables are **built by assemblers**, using symbols exported by the compiler into the assembly-language .s file. An ELF symbol table is contained in the .symtab section. It contains an array of entries. ``

<div align="center">
    <img src="Pic/Symbol table entry.png">
</div>

## Symbol resolution

### Duplicate

At compile time, the compiler exports each global symbol to the assembler as either strong or weak, and the assembler encodes this information implicitly in the symbol table of the relocatable object file. Functions and initialized global variables get strong symbols. Uninitialized global variables get weak symbols.

#### Rules

- Multiple strong symbols with the same name are not allowed.
- Given a strong symbol and multiple weak symbols with the same name, choose the strong symbol.
- Given multiple weak symbols with the same name, choose any of the weak symbols.

### Static libraries

#### Alternatives

- Have the compiler recognize calls to the standard functions and to generate the appropriate code directly.
  - It would add significant complexity to the compiler and would require a new compiler version each time a function was added, deleted, or modified.
- Put all of the standard C functions in a single relocatable object module, that application programmers could link into their executables
  - It would decouple the implementation of the standard functions from the implementation of the compiler.
  - Every executable file in a system would now contain a complete copy of the collection of standard functions, which would be extremely wasteful of disk space. Worse, each running program would now contain its own copy of these functions in memory, which would be extremely wasteful of memory.
  - Any change to any standard function, no matter how small, would require the library developer to recompile the entire source file, a time-consuming operation that would complicate the development and maintenance of the standard functions.

### Algorithms to use static libraries to resolve references

During the symbol resolution phase, the linker scans the relocatable object files and archives left to right in the same sequential order that they appear on the compiler driver’s command line. During this scan, the linker maintains a set E of relocatable object files that will be merged to form the executable, a set U of unresolved symbols (i.e., symbols referred to but not yet defined), and a set D of symbols that have been defined in previous input files. Initially, E, U, and D are empty.

- For each input file f on the command line, the linker determines if f is an object file or an archive. If f is an object file, the linker adds f to E, updates U and D to reflect the symbol definitions and references in f , and proceeds to the next input file.
- If f is an archive, the linker attempts to match the unresolved symbols in U against the symbols defined by the members of the archive. If some archive member m defines a symbol that resolves a reference in U, then m is added to E, and the linker updates U and D to reflect the symbol definitions and references in m. This process iterates over the member object files in the archive until a fixed point is reached where U and D no longer change. At this point, any member object files not contained in E are simply discarded and the linker proceeds to the next input file.
- If U is nonempty when the linker finishes scanning the input files on the command line, it prints an error and terminates. Otherwise, it merges and relocates the object files in E to build the output executable file.

## Relocation

- Relocating sections and symbol definitions: In this step, the linker merges all sections of the same type into a new aggregate section of the same type. The linker then assigns run-time memory addresses to the new aggregate sections, to each section defined by the input modules, and to each symbol defined by the input modules.
- Relocating symbol references within sections. In this step, the linker modifies every symbol reference in the bodies of the code and data sections so that they point to the correct run-time addresses. To perform this step, the linker relies on data structures in the relocatable object modules known as relocation entries.

### Relocation entries

Whenever the assembler encounters a reference to an object whose ultimate location is unknown, it generates a relocation entry that tells the linker how to modify the reference when it merges the object file into an executable.

<div align="center">
    <img src="Pic/Relocation entry.png">
</div>

## Loading executable object files

Loading: The loader copies the code and data in the executable object file from disk into memory and then runs the program by jumping to its first instruction, or entry point.

## Dynamic linking and shared libraries

Shared libraries are “shared” in two different ways.

- In any given file system, there is exactly one .so file for a particular library. The code and data in this .so file are shared by all of the executable object files that reference the library.
- A single copy of the .text section of a shared library in memory can be shared by different running processes.

