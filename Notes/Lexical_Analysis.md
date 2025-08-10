# Lexical Analysis

## Lexical Analysis

Lexical analysis is the first phase of a compiler. Its main task is to read the input characters and group them into sequences called **lexemes**, and then produce as output a sequence of **tokens** for each lexeme.

### Role of Lexical Analyzer
*   **Scanning:** Reads the source code character by character.
*   **Tokenization:** Groups characters into lexemes and converts them into tokens.
*   **Error Handling:** Reports lexical errors (e.g., illegal characters).
*   **Interaction with Parser:** Passes tokens to the parser upon request.

### Tokens, Lexemes, and Patterns
*   **Token:** A pair consisting of a token name and an optional attribute value. The token name is an abstract symbol representing a kind of lexical unit (e.g., `ID` for identifier, `NUM` for number, `REL` for relational operator).
*   **Lexeme:** A sequence of characters in the source program that matches the pattern for a token and is identified by the lexical analyzer as an instance of that token.
*   **Pattern:** A description of the form that the lexemes of a token may take. Patterns are most commonly specified using **regular expressions**.

### Regular Expressions
Regular expressions are a powerful notation for specifying patterns. They are used to define the set of strings that constitute the lexemes for each token.

### Finite Automata (NFA and DFA)
Regular expressions can be converted into finite automata, which are mathematical models of computation used to recognize patterns in strings.
*   **NFA (Nondeterministic Finite Automaton):** Can have multiple transitions for the same input symbol from a state, or transitions on ε (empty string).
*   **DFA (Deterministic Finite Automaton):** For each state and input symbol, there is exactly one transition. DFAs are generally preferred for implementation due to their deterministic nature.

### Conversion from Regular Expression to NFA to DFA
1.  **Regular Expression to NFA:** Thompson's construction algorithm can convert any regular expression into an NFA.
2.  **NFA to DFA:** The subset construction algorithm can convert any NFA into an equivalent DFA.
3.  **DFA Minimization:** Algorithms like Hopcroft's algorithm can minimize the number of states in a DFA while preserving its language recognition capabilities.

### Lexer Implementation Details (Example from `src/lexer/Lexer.java`)
A practical lexer implementation often involves:
*   **Character Reading:** Reading input character by character (e.g., `stream.read()`).
*   **Whitespace and Newline Handling:** Skipping whitespace and incrementing line numbers for newlines.
*   **Comment Handling:** Recognizing and skipping single-line (`//`) and block (`/* ... */`) comments.
*   **Relational Operators:** Recognizing operators like `<=`, `!=`, `>=`, etc. (e.g., `if ("<=!>".indexOf(peek) > -1)`).
*   **Number Recognition:** Parsing integer and floating-point numbers (e.g., checking for digits and decimal points).
*   **Word (Identifier/Keyword) Recognition:** Recognizing sequences of letters and digits.
    *   **Keyword Reservation:** Storing keywords in a symbol table (e.g., `Hashtable<String, Word> words`) to distinguish them from identifiers.
    *   **Identifier Creation:** If a recognized word is not a reserved keyword, it's treated as an identifier.
*   **Token Creation:** Creating `Token` objects (or subclasses like `Num`, `Rel`, `Word`) with appropriate `Tag` values.

### Summary
Lexical analysis is the initial phase of compilation, transforming raw character streams into a sequence of tokens. It involves grouping characters into lexemes based on defined patterns, often specified by regular expressions. These patterns are recognized by finite automata (NFAs and DFAs), which can be systematically constructed from regular expressions. Practical lexers handle various elements like whitespace, comments, numbers, operators, and identifiers, often using a symbol table to manage keywords. The output tokens serve as input for the subsequent parsing phase.