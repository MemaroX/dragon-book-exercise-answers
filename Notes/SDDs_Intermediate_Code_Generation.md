# Syntax-Directed Definitions (SDDs) and Intermediate Code Generation

## Syntax-Directed Definitions (SDDs) and Intermediate Code Generation

This section covers how syntax-directed definitions are used to guide the translation process, particularly in generating intermediate code.

### Syntax-Directed Definitions (SDDs)
An SDD is a context-free grammar where each grammar symbol has an associated set of **attributes**, and each production rule has a set of **semantic rules** for computing the values of these attributes. Attributes can be:
*   **Synthesized Attributes:** The value of a synthesized attribute at a node is computed from the attribute values of its children nodes. They pass information up the parse tree.
*   **Inherited Attributes:** The value of an inherited attribute at a node is computed from the attribute values of its parent, siblings, or itself. They pass information down or across the parse tree.

### Attribute Grammars
*   **S-attributed SDD:** An SDD that uses only synthesized attributes. These can be evaluated by a single bottom-up pass over the parse tree.
*   **L-attributed SDD:** An SDD where attributes can be either synthesized or inherited, but with a restriction: an inherited attribute at a node can only depend on attributes of its parent, or its left siblings. This allows evaluation by a single top-down pass or by a bottom-up pass that simulates a top-down traversal.

### Type Checking
Type checking is the process of verifying that the types of operands in an expression are compatible with the operation being performed. It's a crucial part of semantic analysis.
*   **Type Systems:** A collection of rules for assigning type expressions to parts of a program.
*   **Type Expressions:** A basic type or a type constructed by applying type constructors (e.g., array, record, pointer, function) to other type expressions.
*   **Type Equivalence:** Rules for determining when two type expressions are considered the same.
*   **Overloading:** Allowing a single operator or function name to have different meanings depending on the types of its operands.
*   **Type Conversion (Coercion):** Implicit conversion of a value from one type to another (e.g., `int` to `float`).

### Intermediate Code Generation
Intermediate code generation is a phase in the compiler that translates the source code into an intermediate representation, which is typically machine-independent.
*   **Role:** Bridges the gap between the high-level source code and the low-level target machine code.
*   **Benefits:**
    *   **Portability:** The intermediate representation can be used for different target machines.
    *   **Optimization:** Machine-independent optimizations can be performed on the intermediate code.
    *   **Simplifies Compiler Design:** Divides the compilation process into smaller, manageable phases.

### Forms of Intermediate Code
1.  **Abstract Syntax Trees (ASTs):** A tree representation of the program's structure, abstracting away syntactic details. Nodes represent operations and leaves represent operands.
2.  **Postfix Notation (Reverse Polish Notation):** A linear representation where operators follow their operands. Easy to evaluate using a stack.
3.  **Three-Address Code (TAC):** A sequence of statements, each with at most three operands (two for arguments, one for result). Each statement typically represents a single elementary operation.
    *   **Quadruples:** Each instruction has four fields: `operator`, `operand1`, `operand2`, `result`.
    *   **Triples:** Each instruction has three fields: `operator`, `operand1`, `operand2`. Results are referred to by their position in the triple sequence.
    *   **Indirect Triples:** A list of pointers to triples, allowing for easier reordering of code.
4.  **Directed Acyclic Graphs (DAGs) for Basic Blocks:** A DAG is a graphical representation of a basic block (a sequence of three-address statements with no jumps into or out of the sequence except at the beginning and end). DAGs can identify common subexpressions and eliminate redundant computations.

### Translation of Control Flow Statements
SDDs are used to translate high-level control flow statements into sequences of three-address code with labels and jumps.
*   **`if` statements:** `if (B) S1 else S2` translates to code for `B`, followed by a conditional jump to `S1` or `S2`, and unconditional jumps to skip parts.
*   **`while` loops:** `while (B) S` translates to a label, code for `B`, a conditional jump to the loop body `S`, code for `S`, and an unconditional jump back to the beginning of the loop.
*   **`for` loops:** `for (S1; B; S2) S3` translates to code for `S1`, a label, code for `B`, a conditional jump to `S3`, code for `S3`, code for `S2`, and an unconditional jump back to the label.
*   **Boolean Expressions:** Translated into sequences of conditional and unconditional jumps, often using `truelist` and `falselist` attributes to manage jump targets.

### Array Addressing and Multi-dimensional Arrays
Intermediate code generation for arrays involves calculating memory offsets.
*   For `A[i]`, the address is `base_address + i * element_width`.
*   For multi-dimensional arrays (e.g., `A[i][j]`), the address calculation depends on whether it's row-major or column-major order. For row-major `A[i_1][i_2]...[i_k]`, the offset is typically `((...((i_1 * d_2 + i_2) * d_3 + i_3)...) * d_k + i_k) * element_width`, where `d_x` are the dimensions.

### Symbol Tables
Symbol tables are data structures used by compilers to store information about the identifiers (variables, functions, types, etc.) in a program. They are crucial for type checking and intermediate code generation, providing information like type, scope, and memory allocation details.

### Summary
Syntax-Directed Definitions (SDDs) provide a formal framework for guiding the translation process in compilers. They associate attributes (synthesized for bottom-up information flow, inherited for top-down) with grammar symbols and semantic rules with productions. SDDs are fundamental for tasks like type checking, which ensures type compatibility and handles conversions. The output of this phase is often an intermediate representation, such as Abstract Syntax Trees (ASTs) or Three-Address Code (quadruples, triples, indirect triples), which can be further optimized. Directed Acyclic Graphs (DAGs) are used to optimize basic blocks by identifying common subexpressions. SDDs also facilitate the translation of complex control flow statements and array addressing into this intermediate code, relying on symbol tables to manage program entities.