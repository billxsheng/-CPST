# Java Compilation

- Program stored in files
- Compiler compiles the files and produces corresponding .class files (no linking is done)
- The JVM (Java Virtual Machine) lives in the RAM
    - During execution, class files are brought on the RAM by class loader
    - Byte code is verified (Byte Code Verifier)
    - Execution Engine converts byte code into machine code
    - Just In Time (JIT) Compiling