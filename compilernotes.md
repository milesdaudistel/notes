#Compilers

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
"predicts" but doesn't actually.  just means it always takes the correct production.  never looks at any incorrect productions.  Can do this by taking fewer grammars than recursive descent.

only accept ll(k) grammars.  left to right scan.  looks at the next k tokens.  then it can choose what production based on those k tokens it look at next.  in theory, k can be whatever arbitrary constant.  In reality, it's always 1.  Well, if it only look at the single next token, then how is that any different from recursive descent?

oh, you have first and follow sets because everything is LL(1).  An LL(k) grammar would have a first(k) set and a follow set.  But since we're only bothering with LL(1), it's just first and follow.

left factor the T and E grammar.  T has 2 things that begin with int.  If you only look at the first token, and it's an int, you couldn't choose between those 2 productions.  So you have to left factor.  Left factoring means you make it so that each production for a non-terminal has a unique token or something.

Also how does it work for E?  They both start with T, but on top of that, they're non-terminal, so how can you even say that you can predict if you still have to call all of that stuff?

Although it might work for LL(k), we can always convert to LL(1), so why not do that.
</details>