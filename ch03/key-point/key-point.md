# Chapter 3 Key Points

### 1. Conversion from NFA, DFA to Regular Expressions

http://courses.engr.illinois.edu/cs373/sp2009/lectures/lect_08.pdf

### 2. KMP and its Extended Algorithms (p87)

Refer to matrix's blog post [A Detailed Explanation of the KMP Algorithm](http://www.matrix67.com/blog/archives/115). The article provides examples, which are relatively easy to understand.

### 3. Efficiency of String Processing Algorithms (p103)

For each constructed DFA state, we must construct at most 4|r| new states.

### 4. Time and Space Trade-offs in DFA Simulation (p116)

The algorithm represented in Figure 3-66.

### 5. Minimising the Number of States in a DFA (p115)

Note line 4 of Figure 3-64: "The transitions for states s and t on input a both lead to the same group in Π", not to the same state. If the determination is made based on whether they arrive at the same state, then if the transitions of s and t on input a lead to two different but indistinguishable states, s and t would be considered distinguishable.
