# Chapter 2 Key Points

### 1. Grammars, Syntax-Directed Translation Schemes, and Syntax-Directed Translators

Taking an expression that only supports single-digit addition and subtraction as an example:

1.  **Grammar**

        list -> list + digit | list - digit | digit
        digit -> 0 | 1 | … | 9

2.  **Syntax-Directed Translation Scheme (with left recursion eliminated)**

        expr -> term rest
        rest -> + term { print('+') } rest | - term { print('-') } rest | ε
        term -> 0 { print('0') } | 1 { print('1') } | … | 9 { print('9') }

4.  **Syntax-Directed Translator**

    Java code can be found on p46.

### 2. Abstract Syntax Trees, Parse Trees

Using 2 + 5 - 9 as an example:

![Abstract Syntax Tree and Parse Tree](https://raw.github.com/fool2fish/dragon-book-practice-answer/master/ch02/key-point/assets/dragonbook-keypoint-2.2-2.png)

### 3. Regular Grammars, Context-Free Grammars, Context-Sensitive Grammars?

Grammar Abbreviations:

*   RG: [Regular grammar](http://en.wikipedia.org/wiki/Regular_grammar)
*   CFG: [Context-free grammar](http://en.wikipedia.org/wiki/Context-free_grammar)
*   CSG: [Context-sensitive grammar](http://en.wikipedia.org/wiki/Context-sensitive_grammar)

#### Regular Grammar

[wiki](http://en.wikipedia.org/wiki/Regular_grammar)

In a standard regular grammar, all production rules must satisfy one of the following three forms:

    B -> a
    B -> a C
    B -> epsilon

The key points are:

1.  The left-hand side of a production must be a single non-terminal symbol.
2.  The right-hand side of a production can be empty (epsilon), a single terminal symbol, or a single terminal symbol followed by a single non-terminal symbol.

From a production perspective, this structure ensures that each application of a rule generates zero or one terminal symbol, continuing until the target string is formed.

From a matching perspective, this structure ensures that each application of a rule consumes one non-terminal symbol, continuing until the entire string is matched.

The automaton corresponding to a language defined this way has a specific property: it is a finite state automaton.

Simply put, one only needs to record the current state and receive the next input symbol to determine the next state transition.

#### Regular Grammars and Context-Free Grammars

The biggest difference between a CFG and an RG is that the right-hand side of a CFG's production rules can contain zero or more terminal and non-terminal symbols, with no restrictions on order or quantity.

Imagine a classic example: matching nested parentheses.

    expr -> '(' expr ')' | epsilon

In this production (considering only the first alternative), the right-hand side has a non-terminal `expr` surrounded by terminal symbols. This type of production cannot be standardized into a strict RG. This is an example of a CFG.

The corresponding automaton for this not only needs to record the current state but also the history of how it arrived at the current position to decide the state transition based on the next input symbol. This "history" is essentially a stack storing the matched rules.

The automaton corresponding to a CFG is a PDA (Pushdown Automaton).

The strict rules of an RG have a corresponding benefit: its automaton is very simple, allowing for a highly efficient and straightforward implementation.

#### Context-Sensitive Grammars

A CSG further relaxes the restrictions of a CFG.

The left-hand side of the production can also contain terminal and non-terminal symbols. The terminal symbols on the left-hand side are the source of the "context". This means that during matching, one must not only look at the current match position but also at what surrounds it (the context). The context is not consumed when the rule is applied; it is merely "observed".

The next level above CSG is PSG, phrase-structure grammar.

This basically removes all the restrictions of a CSG.

Both the left and right sides can have any number of terminal and non-terminal symbols in any order.

Since one is unlikely to encounter such grammars outside of natural language processing, we will not go into further detail.

### 4. Why do n levels of operator precedence correspond to n+1 production rules?

The handling of precedence can be resolved at the pure grammar level, or it can be handled by other means within the parser's implementation.

At the pure grammar level, as introduced in the book, for n levels of precedence, you need n+1 production rules.

The grammar for the four basic arithmetic operations introduced in the book causes addition and subtraction to be closer to the root, while multiplication and division are farther away.

The shape of the syntax tree determines the calculation order of the nodes. Nodes farther from the root are processed first. In this way, it appears that multiplication and division are calculated first, meaning they have higher precedence.

Reference: http://rednaxelafx.iteye.com/blog/492667

### 5. What are effective principles for avoiding ambiguous grammars?

The problem of ambiguity is mainly related to the characteristics of CFGs.

The selection structure ("|") of a CFG does not specify an order or priority.
At the same time, multiple rules may share a common prefix.
This is what leads to ambiguity issues.

A PEG is something similar to a CFG, with comparable expressive power for languages.
However, there is no ambiguity at the grammar level because its selection structure ("|") has a defined order or priority.

### 6. How to avoid the infinite loop caused by left-recursive grammars in a predictive parser?

Production rule:

    A -> A x | y

A snippet of pseudocode for a syntax-directed translation:

    void A(){
        switch(lookahead){
            case x:
                A();match(x);break;
            case y:
                match(y):break;
            default:
                report("syntax error")
        }
    }

When a statement matches the `A x` form, the `A()` function will enter an infinite loop. This can be avoided by converting the production rule into an equivalent non-left-recursive form:

    B -> y C
    C -> x C | ε

### 7. Why is it more difficult to translate expressions with left-associative operators in a right-recursive grammar?

### 8. The issue of l-values and r-values during intermediate code generation.

Looking at the pseudocode for `lvalue()` and `rvalue()` in the book, it feels like anything that can be either an l-value or an r-value is handled by `lvalue()`. As for handling r-values, either they are handled directly, or for r-values that can also be l-values, `lvalue()` is called.

Why not just have a single `value()` function to settle it?