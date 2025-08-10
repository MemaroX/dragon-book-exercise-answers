# Parsing Techniques

## Parsing Techniques

Parsing is the second phase of a compiler, following lexical analysis. Its main task is to take the sequence of tokens produced by the lexical analyzer and build a syntax tree (or parse tree) for them, verifying that the sequence of tokens conforms to the grammatical rules of the language.

### Role of Parser
*   **Syntax Analysis:** Checks if the sequence of tokens forms a valid sentence in the language according to its grammar.
*   **Tree Construction:** Builds a hierarchical representation of the input, typically a parse tree or an Abstract Syntax Tree (AST).
*   **Error Reporting:** Reports syntax errors.
*   **Interaction with Semantic Analyzer:** Provides the syntax tree for further analysis.

### Grammars
Grammars are formal systems that describe the syntax of a language.
*   **Context-Free Grammars (CFG):** The most common type of grammar used in compiler design. Production rules have a single non-terminal on the left-hand side.
    *   Example: `expr -> term rest`
*   **Regular Grammars (RG):** A restricted form of CFG, typically used to define lexical structures. Production rules are limited to specific forms (e.g., `B -> a` or `B -> a C`). RGs correspond to Finite Automata.
*   **Context-Sensitive Grammars (CSG):** More powerful than CFGs, allowing context to influence production rules. Less commonly used in practical compiler design due to increased complexity.

### Parse Trees vs. Abstract Syntax Trees (ASTs)
*   **Parse Tree (Concrete Syntax Tree):** A full hierarchical representation of the input string according to the grammar. It includes all non-terminals and terminals from the derivation.
*   **Abstract Syntax Tree (AST):** A condensed, abstract representation of the program's structure. It omits unnecessary details from the parse tree (like intermediate non-terminals) and focuses on the essential structural elements.

### Top-Down Parsing
Top-down parsers build the parse tree from the root down to the leaves. They try to find a leftmost derivation of the input string.
*   **Predictive Parsers:** A type of top-down parser that can determine the production to apply by looking only at the next input symbol.
*   **Handling Left Recursion:** Direct left recursion (e.g., `A -> Aα | β`) can cause infinite loops in predictive parsers. It must be eliminated by rewriting the grammar into an equivalent non-left-recursive form (e.g., `A -> βA'`, `A' -> αA' | ε`).

### Bottom-Up Parsing
Bottom-up parsers build the parse tree from the leaves up to the root. They try to find a rightmost derivation in reverse.
*   **Shift-Reduce Parsers:** A common type of bottom-up parser that uses a stack and performs two main actions:
    *   **Shift:** Pushes the next input symbol onto the stack.
    *   **Reduce:** Replaces a handle (a right-hand side of a production) on top of the stack with its corresponding non-terminal.
*   **LR Parsers (Left-to-right scan, Rightmost derivation):** A powerful class of shift-reduce parsers. They are widely used because they can parse a large class of context-free grammars and can detect syntax errors as early as possible.
    *   **LR(0):** The simplest form, makes decisions based only on the state. Can have shift-reduce or reduce-reduce conflicts.
    *   **SLR (Simple LR):** Improves LR(0) by using FOLLOW sets to resolve reduce conflicts.
    *   **LALR (Look-Ahead LR):** More powerful than SLR, but less powerful than canonical LR. It merges LR(0) states that have the same core, which can introduce new reduce-reduce conflicts. It is widely used in practice due to its balance of power and table size.
    *   **Canonical LR (LR(1)):** The most powerful LR parser, uses 1-symbol lookahead to resolve all conflicts that can be resolved by LR parsing. However, it generates very large parsing tables.

### Ambiguity
A grammar is **ambiguous** if there exists at least one string that has more than one parse tree (or more than one leftmost/rightmost derivation). Ambiguity is problematic as it leads to multiple interpretations of the source code.
*   **Eliminating Ambiguity:** Often involves rewriting the grammar to enforce operator precedence and associativity, or by introducing new non-terminals. (Refer to the "Ambiguous Grammars" note for more details).

### Operator Precedence and Associativity
These concepts are crucial for correctly parsing expressions.
*   **Precedence:** Defines the order in which operators are evaluated (e.g., multiplication usually has higher precedence than addition). Grammars can enforce precedence by using multiple levels of non-terminals (e.g., `Expr -> Expr + Term | Term`, `Term -> Term * Factor | Factor`).
*   **Associativity:** Defines how operators of the same precedence are grouped (e.g., left-associative for `+` and `-` means `a - b - c` is parsed as `(a - b) - c`). Left-associativity is typically handled by left-recursive productions, while right-associativity by right-recursive productions.

### Chomsky Normal Form (CNF) and Backus-Naur Form (BNF)
*   **BNF (Backus-Naur Form):** A metasyntax used to express context-free grammars. It is a common way to write grammar rules.
*   **CNF (Chomsky Normal Form):** A specific type of normal form for context-free grammars where all production rules are of the form `A -> BC` (where A, B, C are non-terminals) or `A -> a` (where a is a terminal). Any CFG can be transformed into CNF.

### Summary
Parsing is the compiler phase that validates token sequences against grammar rules and builds a syntax tree. It relies on formal grammars like Context-Free Grammars. Parsers can be top-down (e.g., predictive parsers, which require left recursion elimination) or bottom-up (e.g., powerful LR parsers like SLR, LALR, and LR(1)). A key challenge is handling ambiguous grammars, which allow multiple interpretations; this is resolved by enforcing operator precedence and associativity. Grammars are often expressed in BNF, and can be converted to normal forms like CNF for theoretical analysis or specific parsing algorithms.