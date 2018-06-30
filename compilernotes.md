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

$\alpha$


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

<details><summary></summary>

</details>

<details><summary></summary>

</details>

<details><summary>Predictive Parsing</summary>
predictive parsing means you left factor the grammar so that you never have to backtrack.  predictive parsing is a bad name because a prediction is a guess, but this thing is completely deterministic just like a normal recursive descent parser.  In fact, it's just as dumb, too.  It doesn't perform better because of a better algorithm, it performs better because it only takes ll(k) grammars.  Starts at the left, looks at the next k tokens.  In practice, it's always ll(1).  

ll(k) uses first(k) sets and follow(k) sets, but since we only ever care about ll(1) grammars, it's just first and follow sets.  

left factoring

Backtracking means you have to go back after you have matched tokens.  Calling E, which then calls T, which then looks for an int, and the int not matching, that is not backtracking, because you haven't matched anything yet.  It's only backtracking if you've matched an int, then move on to something else, then realize that you need to undo that match.  Maybe an example would work best.  Think of something that almost gets there, but then needs to go back after all its work.

What are first and follow set for?
T[A, t] = ?
This means if we are currently looking at nonterminal A, and t is the next input token, what will we transition to?  The reason we want to know this is so we can build an LL(1) parsing table.  Need to look at recursive descent parsing table in order to understand how parsing tables work.
</details>