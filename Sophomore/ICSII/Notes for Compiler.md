# <center>Notes for Compiler</center>

[TOC]

Analysis

- **Lexical analysis**: breaking the input into individual words or "tokens";
- **Syntax analysis**: parsing the phrase structure of the program; and
- **Semantic analysis**: calculating the program's meaning

## Regular Expressions & Finite Automata

### Two important disambiguation rules of Regular Expressions

- **Longest match**: The longest initial substring of the input that can match any regular expression is taken as the next token.
- **Rule priority**: For a particular longest initial substring, the first regular expression that can match determines its token type.

### Finite Automata

### Nondeterministic Finite Automata

#### Converting a Regular Expression to an NFA

<div>
    <img src="Pic/Regular to NFA.png">
</div>

#### Converting an NFA to a DFA

<div align="center">
    <img src="Pic/Algorithm for NFA to DFA.png"> 
</div>

<div align="center">
    <img src="Pic/A_star.png">
</div>

## Parsing

### Predictive Parsing

#### First and Follow sets

- **nullable(X)** is true if X can derive the empty string.
- **FIRST($\gamma$)** is the set of terminals that can begin strings derived from $\gamma$.
- **FOLLOW(X)** is the set of terminals that can immediately follow X. That is, t $\in$ FOLLOW(X) if there is any derivation containing $X_t$. This can occur if the derivation contains XYZt where Y and Z both derive $\epsilon$.
- **Algorithm**

    <div align="center">
        <img src="Pic/Algorithm to derive sets.png">
    </div>

#### Predictive parsing table

Enter production $X \rightarrow \gamma$ in row X, column T of the table for each $T \in FIRST(\gamma)$. Also, if $\gamma$ is nullable, enter the production in row X, column T for each $T \in FOLLOW[X]$. An example below may help understand.

<div align="center">
    <img src="Pic/Grammar.png">
    <img src="Pic/sets.png">
    <img src="Pic/table.png">
</div>

As we can see, some of the entries contain more than one production, which indicates that predictive parsing will not work on this grammar.

#### Ambiguity

No ambiguous grammar is LL(k) for any k, which means there are no duplicate entries in the predictive parsing tables.

##### Left recursion

For productions

​				$E\rightarrow E+T \\E\rightarrow T$

Clearly, we can see that any token in FIRST(T) will also be in FIRST(E+T). So it can not be the LL(1) (without duplicate entries in the predictive parsing tables).

To eliminate left recursion, we can rewrite them using right recursion.

​				$E \ \rightarrow TE'\\E'\rightarrow +TE'\\E'\rightarrow$

##### Left factoring

​				S $\rightarrow$ if E then S else S 

​				S $\rightarrow$ if E then S

We can left factoring the grammar - that is, take the allowable endings and make a new nonterminal X to stand for them:

​				S $\rightarrow$ if E then S X

​				X $\rightarrow$

​				X $\rightarrow$ else S

#### Weakness

LL(k) must predict which production to use, having seen only the first k tokens of the right-hand side.

### LR Parsing

<div align="center">
    <img src="Pic/Algorithm for LR0_1.png">
    <img src="Pic/Algorithm for LR0_2.png">
</div>

Example

<div align="center">
    <img src="Pic/Grammar for LR0.png">
    <img src="Pic/DFA for LR0.png">
    <img src="Pic/Parsing table for LR0.png">
</div>

As for constructing a parsing table, we put the action shift J at position(I, X) for each edge $I \rightarrow^X J$, where X is a terminal; if X is a nonterminal we put goto J at position(I, X).

### Other Parsing

#### SLR Parsing

We only put reduce actions at position (I, X) when token X in FOLLOW(A) where A $\rightarrow \alpha.$ belongs to state I.

#### LR(1)

We only put reduce actions at position (I, X) when token X is the lookahead belonging to state I.

<div align="center">
    <img src="Pic/LR1.png">
</div>

#### LALR(1)

Merge any two states whose items are identical except for lookahead sets.

<div align="center">
    <img src="Pic/Table for LR1 and LALR1.png">
</div>





