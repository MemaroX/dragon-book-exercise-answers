# Chapter 4 Key Points

### ! The difference between LR(0), SLR, LR, and LALR

p157: How does an LR(0) automaton make shift-reduce decisions? Assume a grammar symbol string γ causes the LR(0) automaton to run from the start state 0 to some state j. Then, if the next input symbol is a and state j has a transition on a, shift a; otherwise, perform a reduction.

This method can lead to some incorrect reductions. Suppose the reduced symbol is X, but a is not in FOLLOW(X); this situation will cause problems. Therefore, SLR made improvements in this regard.

p161: When constructing an SLR parsing table, if [A -> α.] is in I_i, then for all a in FOLLOW(A), set ACTION[i, a] to "reduce A -> α".

SLR solves the problem of incorrect reductions to a certain extent, but not completely. This is because although a reduction is chosen only when a is in FOLLOW(A), for the current state I_i, not every terminal in FOLLOW(A) can appear after A in state I_i.

p166: To put it more formally, it is necessary to specify precisely for I_i which input symbols can follow the handle α, so that α can be reduced to A.

LR solves this problem by adding a second component to the item, the lookahead symbol. But the new problem is that LR can make the state table extremely large, whereas LALR is a more economical approach that has as many states as SLR.

p170: Generally speaking, by merging sets of LR items that have the same set of core items, one can obtain the LALR item sets. Although LALR may perform some incorrect reductions, it will ultimately discover this error before any new symbols are input.

### Eliminating Ambiguity (p134)

Figure 4-10, how is this elimination method derived?

### Eliminating Left Recursion (p135)

Why can the algorithm in Figure 4-11 eliminate left recursion in a grammar?

Eliminating recursion requires satisfying two conditions:

1.  There is no immediate left recursion, i.e., no productions of the form A -> Aα.
2.  There is no left recursion that can be produced by a multi-step derivation.

The result of the loop in lines 3-5 of the algorithm ensures that productions of the form A_i -> A_m α must satisfy m >= i. This eliminates the possibility of transitions like S => Aa => Sda, which means that a production starting with A_i can definitely not be derived from A_m. Therefore, A_m α does not have the possibility of producing A_i left recursion.

**It should also be noted:** It is only necessary to handle productions like A_i -> A_j α, and not productions of the form A_i -> α A_j β.

After the loop is completed, line 6 eliminates the immediate left recursion in the substituted productions.

### Using LR(0) to create the kernel of LALR(1) item sets (p173)

Spontaneously generated and propagated lookahead symbols.

### CNF and BNF

- [Chomsky normal form](http://en.wikipedia.org/wiki/Chomsky_normal_form)
- [Backus Naur Form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_Form)