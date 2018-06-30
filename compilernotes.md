#Compilers

<details><summary>Notation</summary>
We'll start off with this example:

	E -> T | T + E
	T -> int | int * T | (E)

`grammar` the whole thing

`E, T` nonterminals

`int, *, (, )` terminals

`E -> T` the first production of E

`T -> int * T` the second production of T

`|` used to separate the productions of the nonterminals

`->` E can be converted to T

`->*` X can be converted to Y in ? steps

</details>

<details><summary>DFA to Table Driven</summary>

</details>

<details><summary>Recursive Descent</summary>
Easiest type of parsing.  Takes almost any context free grammar (left recursive and something else don't work without some kind of modification).

Example Grammar

	E -> T | T + E
	T -> int | int * T | (E)
	
Example Functions

	bool Term(Token tok) { return *next++ == tok; }
	
	bool E() { 
		Token *save = next;
		
	
	bool E_1() { return T(); }
	bool E_2() { return T() && term('+') && E(); }
	
	bool T_1() { return term(INT); }
	bool T_2() { return term(INT) && term('*') && T(); }
	bool T_3() { return term('(') && E() && term(')'); }
	

</details>



<details><summary>Predictive Parsing and LL(k) grammars</summary>
Predictive parsing is a confusing term.  Saying that you have a 'predictive parser' is not a statement about your parsing algorithm (recursive descent, shift reduce, etc).  Saying that you have a predictive parser means that the grammar your parser reads is an LL(k) grammar.  LL(k) grammars are a special kind of context-free grammars.  By looking at the next k tokens, we can narrow down the possible productions to 1 at every step.  This means there will never be any backtracking.

Here is an example of a normal context-free grammar:

	A -> aaaa | aaab
	
Let's say we get the input `aaab`.  This grammar starts at the first production of A, matchs the first, second and third `a`, then hits `b` and has to backtrack.  Starting over at the first `a`, the parser matches the input with the second production, and we're done.

Here's an LL(k) grammar that parses the same language as above:

	A -> aaaX
	X -> a | b
	
In this grammar, k=1 because you only have to look at the next token to decide whether or not to keep parsing or error out.  For our input `aaab`, we match the first 3 `a`'s one at a time, then look at X.  `b` doesn't match X's first production, so we go to X's second production and get a match.  This is better than the first grammar because we only had to match `aaa` once.

So LL(k) grammars don't have to backtrack, unlike most context-free grammars.

All context-free grammars have an LL(k) equivalent.  Tools like ANTLR can transform these grammars automatically.

TODO explain left factoring, that other example of stuff that won't work with normal recursive descent.
first and follow sets.  ll(k) uses first(k) sets and follow(k) sets, but since we only ever care about ll(1) grammars, it's just first and follow sets.  

What are first and follow set for?
T[A, t] = ?
Need to explain parsing tables in general.  I think he explained them for regular grammars.
</details>


<details><summary>LL(k) vs regular grammars</summary>
Consider this grammar that parses nested parenthesis:

	E -> (E) | epsilon

This isn't something you can describe with a regular grammar.  So LL(k) grammars are more general that regular grammars.  

LL(k) grammars can be parsed in linear time just like regular grammars, unlike non-LL(k) context-free grammars.

I would say use regular grammars when you can, as they are more convenient, and use LL(k) grammars when you have to.
</details>