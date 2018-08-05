<details><summary>DFA To Table Driven, or How Regex Works Under The Hood</summary>
Consider this regex of all binary strings that end in 1:

	(1|0)*1

Whose DFA implementation is as follows:	

![endsin1](pics/endsin1.png)

We can convert this DFA into a table.  A table is easy to implement in code, as well as fast.  Here is a table equivalent to the DFA:

|  | `0` | `1` |
|-----|-----|-----|
| `S` | T | U |
| `T` | T | U |
| `U` | T | U |
 
 Say we have the input `0101`.  We are in state S, and the next input is 0, so we look at row S, column 0, whose entry is T.  This means we are now in state T.  
 
 The next input is 1, so we look in row T, column 1, whose entry is U.  Now we are in state U.  
 
 The next input is 0, so we look in row U, column 0, whose entry is T.  Now we are in state T.
 
 The next input is 1, so we look in row T, column 1, whose entry is U.  Now we are in state U.
 
 There is no more input, so we are done.  One thing that isn't in table that we need to somehow specify is that U is an accepting state, and S and T aren't.  Implemention is trivial and will be omitted :^)
 
 So in general, you have a current state, and you have a next token of input.  You do a table look up based on those 2 things to find your next state.  If at any point you try to do a table lookup and find nothing, that means the string does not match.  If we had the string `ab`, we would look in row S, and we wouldn't find a column corresponding to `a`, and our algorithm would return false, saying the string does not match our regular expression.

</details>

<details><summary>Recursive Descent</summary>
Recursive Descent is the simplest type of parsing algorithm.  The way recursive descent works is we get a big list (or stream) of tokens from the lexer.  We look at these tokens one at a time, forming them into a tree.

Example Grammar for recursive descent:

	E -> T | T + E
	T -> int | int * T | (E)
	
Recursive Descent Functions:

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
Predictive parsing is a confusing term.  Saying that you have a 'predictive parser' is not a statement about your parsing algorithm (recursive descent, shift reduce, etc).  Saying that you have a predictive parser means that the grammar your parser reads is an LL(k) grammar.  LL(k) grammars are a special kind of context-free grammars.  By looking at the next k tokens, we can narrow down the possible productions to 1 at every step.  This means there will never be any backtracking, making it faster.

Here is an example of a normal context-free grammar:

	A -> aaaa | aaab
	
Let's say we get the input `aaab`.  This grammar starts at the first production of A, matchs the first, second and third `a`, then hits `b` and has to backtrack.  Starting over at the first `a`, the parser matches the input with the second production, and we're done.

Here's an LL(k) grammar that parses the same language as above:

	A -> aaaX
	X -> a | b
	
In this grammar, k=1 because you only have to look at the next token to decide whether or not to keep parsing or error out.  For our input `aaab`, we match the first 3 `a`'s one at a time, then look at X.  `b` doesn't match X's first production, so we go to X's second production and get a match.  This is better than the first grammar because we only had to match `aaa` once.

So LL(k) grammars don't have to backtrack, unlike most context-free grammars.

Random facts:

All context-free grammars have an LL(k) equivalent.  Tools like ANTLR can transform context-free grammars into LL(k) grammars automatically.

In practice, we'll always be looking at LL(1) grammars. LL(k>1) grammars don't matter.  I think it's because if you can convert it to an LL(k) grammar, you can convert it to an LL(1) grammar.  LL(1) grammars are either faster, or simpler than any other value of k.  Something like that.

</details>

<details><summary>LL(k) vs regular grammars</summary>
Consider this grammar that parses nested parenthesis:

	E -> (E) | epsilon

Regular grammars can't describe nested parens.  So LL(k) grammars are more general that regular grammars.  

LL(k) grammars can be parsed in linear time just like regular grammars, unlike non-LL(k) context-free grammars.

The only advantage of regular grammars is that they can be described by regular expressions, and so are simpler to write out than context free grammars.
</details>

<details><summary>LL(1) Parsing tables</summary>

Remember how we converted DFA's into tables?  Tables are simple to implement in code, and fast to execute.  Now we want to make a parsing table for LL(1) grammars.

`Structure and usage of parsing table`

Lets say we have this grammar:

	E -> TX
	T -> (E) | int Y
	X -> +E | epsilon
	Y -> *T | epsilon

Its parsing table will look like this:

|   | `(` | `)` | `+` | `*` | `int` | `$` |
|---|-----|---------|---------|----|-------|---------|
| `E` | TX |  |  |  | TX |  |
| `T` | (E) |  |  |  | int Y |  |
| `X` |  | epsilon | +E |  |  | epsilon |
| `Y` |  | epsilon | epsilon | *T |  | epsilon |

Blanks in the table mean error.

We can create a parse tree using this table by starting at E, and looking at the first terminal in our input, we do a table lookup.  Whatever we find, we add to our tree with E as the root.  Since it's a leftmost derivation, we travel the branches in a pre-order fashion until we hit a leaf node that is a non-terminal.  Then we look at the next terminal, and do another table look up.

Input stream:

	(3 * 4) + 2

Derivation:

	E -> T X -> (E) X -> (T X) X -> (int Y X) X -> (int * T X) X -> (int * int Y X) X -> (int * int X) X 
	-> (int * int) X -> (int * int) + E -> (int * int) + T X -> (int * int) + int Y X -> (int * int) + int X
	-> (int * int) + int

These derivation steps are going to be gone through using the table.
TODO might want to put more captions on each of these steps so that people get a better idea of how it corresponds to the table.

<details><summary>Step-by-step derivation</summary>

<a class="prev" onclick="plusSlides2(-1)">&#10094;</a>
<a class="next" onclick="plusSlides2(1)">&#10095;</a>

<div class="mySlides2">
<div>1 / 14</div>
<pre>
(3*4)+2
&#8593;
</pre>
<img src="pics/ETXY1.png">
</div>

<div class="mySlides2">
<div>2 / 14</div>
<pre>
(3*4)+2
&#8593;
</pre>
<img src="pics/ETXY2.png">
</div>

<div class="mySlides2">
<div>3 / 14</div>
<pre>
(3*4)+2
 &#8593;
</pre>
<img src="pics/ETXY3.png">
Now that we've derived the '(', we can move on to the second token of input, '3'.
</div>

<div class="mySlides2">
<div>4 / 14</div>
<pre>
(3*4)+2
 &#8593;
</pre>
<img src="pics/ETXY4.png">
</div>

<div class="mySlides2">
<div>5 / 14</div>
<pre>
(3*4)+2
  &#8593;
</pre>
<img src="pics/ETXY5.png">
</div>

<div class="mySlides2">
<div>6 / 14</div>
<pre>
(3*4)+2
   &#8593;
</pre>
<img src="pics/ETXY6.png">
</div>

<div class="mySlides2">
<div>7 / 14</div>
<pre>
(3*4)+2
    &#8593;
</pre>
<img src="pics/ETXY7.png">
</div>

<div class="mySlides2">
<div>8 / 14</div>
<pre>
(3*4)+2
    &#8593;
</pre>
<img src="pics/ETXY8.png">
Since the row 'Y', column ')' entry is 'epsilon', Y is nothing, and we do not need to represent it anymore.
</div>

<div class="mySlides2">
<div>9 / 14</div>
<pre>
(3*4)+2
     &#8593;
</pre>
<img src="pics/ETXY9.png">
After we delete the X that turned out to be an epsilon, we hit the ')', and consume it.  Only writing this because it may not be immediately clear.
</div>

<div class="mySlides2">
<div>10 / 14</div>
<pre>
(3*4)+2
     &#8593;
</pre>
<img src="pics/ETXY10.png">
</div>

<div class="mySlides2">
<div>11 / 14</div>
<pre>
(3*4)+2
     &#8593;
</pre>
<img src="pics/ETXY11.png">
</div>

<div class="mySlides2">
<div>12 / 14</div>
<pre>
(3*4)+2
      &#8593;
</pre>
<img src="pics/ETXY12.png">
</div>

<div class="mySlides2">
<div>13 / 14</div>
<pre>
(3*4)+2
      &#8593;
</pre>
<img src="pics/ETXY13.png">
</div>

<div class="mySlides2">
<div>14 / 14</div>
<pre>
(3*4)+2
      &#8593;
</pre>
<img src="pics/ETXY14.png">
Here is our finished parse tree.  We can turn it into an AST by getting rid of all the non-terminals, which I don't feel like doing.
</div>

</details>

`How parsing tables are constructed`

Looking at our parsing table again, we know that if our current non-terminal is 'T', and our current terminal is '(', we should choose the production '(E)', and if an 'int' is our terminal, we should choose the production 'int Y'.  What we don't know is how to make a parsing table like this.  How should we know to assign T['T', '('] -> '(E)' in the first place?  Now we will show you.

In the DFA table, we had a current state, and a next token which would determine our next table lookup.  In our LL(1) table, we have a current non-terminal instead of a current state.  Each table lookup has this form:

	T[A, t] = X
	
TODO how is the parsing table really created?  What 2 things do we have, what 3rd thing are we looking for?  Iterate over each position in the table?  Iterate over all possible terminals?  And why can we have 2 things (like a TX) in the parsing table?

So we have our parsing table with non-terminals as rows and terminals as columns, and nothing in the entries.  For each non-terminal, we iterate over its productions.  For each production, we calculate that productions first set using the first and follow sets of each thing it contins.  Then we look at that productions first set.  For each terminal in the productions first set, we put that production in at the entry (non-terminal, terminal).

<details><summary>First Sets</summary>
`T[A, t] = B` if t is in the first set of B.  
The first set of a non-terminal B is the set of all terminals that appear first in B's derivation.

	A -> Bx | Cy
	B -> 0 | 1
	C -> a | epsilon
	
In this grammar, the first sets of A, B and C are:

	A : { 0, 1, a, y }
	B : { 0, 1 }
	C : { a, epsilon }
	
B and C are trivial, so I'll skip those.  A's first set looks like it does because when A is derived all the way down to terminals, it could look like any of the following:

	0x
	1x
	ay
	y
	
Since we want the first terminal that can be derived from A, we end up with 0, 1, a, and y.

So if we're at A, we will transition to B if `t=0` or `t=1`.
TODO I this is wrong.  What happens after we get to B?  We just die.

In general, finding the first sets for each terminal and non-terminal in a grammar is as follows:

	t : { t } // if t is a terminal symbol
	First(Y) is a subset of First(X) if X -> Y....
		or X -> ABCY....
			and A, B, C can all be epsilon.
	epsilon is an element of First(X) if:
		X -> epsilon
		or
		X -> ABC
			and A, B, C can all become epsilon

</details>

<details><summary>Follow Sets</summary>
Here's the definition of a follow set for non-terminal X:

	Follow(X) = { t | Y ->* AXtB }
	
What this says is that t is in Follow(X) if Y's multiple-step derivation contains X, and also has some terminal t after it.  

The start symbol S's follow set will contain only `$` (end of file).

<a class="prev" onclick="plusSlides1(-1)">&#10094;</a>
<a class="next" onclick="plusSlides1(1)">&#10095;</a>

<div class="mySlides1">
Step 1 / 14:  Start at E
<pre>
Follow(E) = { $ }
Follow(T) = { }
Follow(X) = { }
Follow(Y) = { }
Follow('(') = { }
Follow(')') = { }
Follow('+') = { }
Follow('*') = { }
Follow(int) = { }
</pre>
</div>

<div class="mySlides1">
Step 2 / 14:  Look at E -> TX
<pre>
Follow(E) = { $ }
Follow(T) = { First(X) }
Follow(X) = { Follow(E) }
Follow(Y) = { }
Follow('(') = { }
Follow(')') = { }
Follow('+') = { }
Follow('*') = { }
Follow(int) = { }
</pre>
</div>

<div class="mySlides1">
Step 3 / 14:  Look at T -> (E)
<pre>
Follow(E) = { $, ')' }
Follow(T) = { First(X) }
Follow(X) = { Follow(E) }
Follow(Y) = { }
Follow('(') = { First(E) }
Follow(')') = { Follow(T) }
Follow('+') = { }
Follow('*') = { }
Follow(int) = { }
</pre>
</div>

<div class="mySlides1">Step 4 / 14:  Look at T -> int Y
<pre>
Follow(E) = { $, ')' }
Follow(T) = { First(X) }
Follow(X) = { Follow(E) }
Follow(Y) = { Follow(T) }
Follow('(') = { First(E) }
Follow(')') = { Follow(T) }
Follow('+') = { }
Follow('*') = { }
Follow(int) = { First(Y) }
</pre>
</div>
	
<div class="mySlides1">Step 5 / 14:  Look at X -> +E
<pre>
Follow(E) = { $, ')', Follow(X) }
Follow(T) = { First(X) }
Follow(X) = { Follow(E) }
Follow(Y) = { Follow(T) }
Follow('(') = { First(E) }
Follow(')') = { Follow(T) }
Follow('+') = { First(E) }
Follow('*') = { }
Follow(int) = { First(Y) }
</pre>
</div>


<div class="mySlides1">Step 6 / 14:  Look at X -> epsilon
<pre>
Follow(E) = { $, ')', Follow(X) }
Follow(T) = { First(X) }
Follow(X) = { Follow(E) }
Follow(Y) = { Follow(T) }
Follow('(') = { First(E) }
Follow(')') = { Follow(T) }
Follow('+') = { First(E) }
Follow('*') = { }
Follow(int) = { First(Y) }
</pre>
</div>

<div class="mySlides1">Step 7 / 14:  Look at Y -> * T
<pre>
Follow(E) = { $, ')', Follow(X) }
Follow(T) = { First(X), Follow(Y) }
Follow(X) = { Follow(E) }
Follow(Y) = { Follow(T) }
Follow('(') = { First(E) }
Follow(')') = { Follow(T) }
Follow('+') = { First(E) }
Follow('*') = { First(T) }
Follow(int) = { First(Y) }
</pre>
</div>

<div class="mySlides1">Step 8 / 14:  Look at Y -> epsilon
<pre>
Follow(E) = { $, ')', Follow(X) }
Follow(T) = { First(X), Follow(Y) }
Follow(X) = { Follow(E) }
Follow(Y) = { Follow(T) }
Follow('(') = { First(E) }
Follow(')') = { Follow(T) }
Follow('+') = { First(E) }
Follow('*') = { First(T) }
Follow(int) = { First(Y) }
</pre>
</div>

<details><summary>TODO algorithm for computing follow sets</summary> 
You should just ignore this part for now.  It's not necessary to know how to make these first and follow sets.  Just know how to recognize them.  It's a complicated exponential runtime algorithm.
Here is the general algorithm for getting follow sets, which we run on each production: 

	for each  nonterminal on the right side of X -> ABC....Z:
		Follow(A) += First(B)
		if epsilon in First(B)
			Follow(A) += First(C)
			if epsilon in First(C)
				....
				if epsilon in First(Z)
					Follow(A) += Follow(X)
					
		Follow(B) += First(C)
		if epsilon in First(C)
				....
				if epsilon in First(Z)
					Follow(B) += Follow(X)
					
		Do this for all of them
		TODO should make a recursive version of this.  Also should have a portion that takes into account whether First(B) is already in Follow(A), etc.
		


Now we'll compute follow sets for this grammar, where E is the start symbol:

	E -> TX
	T -> (E) | int Y
	X -> +E | epsilon
	Y -> *T | epsilon

<details><summary>computing follow sets example</summary>
In this example, keep in mind the first sets from the previous example.


This is really long, so I'm not going to finish it.  It would have been nice to have the ability to do slideshows.  Stupid markdown.


</details>

</details>
	
</details>

<details><summary>building the parsing table after you have first and follow sets</summary>

TODO potentially put the first follow algorithms + examples here instead.
TODO look at video LL1 parsing tables again.  The `T[A, t] = B` thing. How do you know what the next A is?  It's actually the leftmost nonterminal in your current derivation.  Going to have to go back and change that.  If the leftmost thing is a terminal, then `T['int', t]` is a pointless lookup, since you can just do a direct comparison:  `'int' == t`.  For simplicity you could do a table lookup, or maybe this is faster.  Whatever.  Just do what you think would be easier to explain.

Now that we have the first and follow sets for each terminal and non-terminal, we can find all the t's for each `T[A, t] = B`.  

	for all non-terminal combos A and B in your language:
		for each t in First(B):
			T[A, t] = B
		if epsilon in First(B):
			for each t in Follow(A):
				T[A, t] = B

</details>

</details>





<script>
var slideIndex = 1;
showSlides1(slideIndex);

function plusSlides1(n) {
  showSlides1(slideIndex += n);
}

function showSlides1(n) {
  var i;
  var slides = document.getElementsByClassName("mySlides1");
  if (n > slides.length) {slideIndex = 1}    
  if (n < 1) {slideIndex = slides.length}
  for (i = 0; i < slides.length; i++) {
      slides[i].style.display = "none";  
  }
  slides[slideIndex-1].style.display = "block";
}
</script>

<script>
var slideIndex = 1;
showSlides2(slideIndex);

function plusSlides2(n) {
  showSlides2(slideIndex += n);
}

function showSlides2(n) {
  var i;
  var slides = document.getElementsByClassName("mySlides2");
  if (n > slides.length) {slideIndex = 1}    
  if (n < 1) {slideIndex = slides.length}
  for (i = 0; i < slides.length; i++) {
      slides[i].style.display = "none";  
  }
  slides[slideIndex-1].style.display = "block";
}
</script>

<details><summary>How does the parser decide whether to shift or reduce?  For example, why does step 2 -> 3 do a Shift, rather than Reduce by Production 1: T -> int?</summary>
Let's see what happens if we did reduce by production 1 rather than shifting.  Rather than some mysterious way of shifting and reducing, we'll reduce whenever it's possible.

	Step 1:   | int * int + int
	Step 2:   int | * int + int
	Step 3:   T | * int + int
	Step 4:   E | * int + int
	Step 5:   E * | int + int
	Step 6:   E * int | + int
	Step 7:   E * T | + int
	Step 8:   E * E | + int
	Step 9:   E * E + | int
	Step 10:  E * E + int |
	Step 11:  E * E + T |
	Step 12:  E * E + E |
	
And now we're stuck.  There's nothing left to shift, and we also can't reduce anything.  Since we couldn't reduce to a single start symbol E, we don't know in what order to execute * or +, which means we can't create a binary for the input program.  How the parser actually knows whether to shift or reduce is a pretty complex topic, and will be covered later.
</details>

Why does the table say to shift at this step?  Why can't we reduce at this step?
Let's see what would happen if rather than using the table, we reduced as much as possible, shifting only when there was nothing left to reduce.
	asdfasdf
Ok, now we're stuck.  So we can't just reduce whenever possible.  The purpose of the table is to tell us exactly what action to take, whether it's a shift or reduce.  Without the table, we would have to guess at which production to use next.  We could potentially use the wrong production and get stuck.

<details><summary>PostgreSQL</summary>
PostgreSQL is an extension of SQL.  There are many extensions for SQL like MySQL and SQLite, but PostgreSQL is considered the most modular and advanced one.  For instance, it has support for nesting, which no other SQL implementation has.

<details><summary>Creating a table</summary>

	CREATE TABLE weather (
	    city            varchar(80),
	    temp_lo         int,           -- low temperature
	    temp_hi         int,           -- high temperature
	    prcp            real,          -- precipitation
	    date            date
	);

Here we make a new table called weather.  City, temp\_lo, temp\_hi, prcp, and date are column names, to the right are their types, and everything about the -- are comments.  Note that date is a type, as well as a name.  Also note that white space doesn't matter, this could all be on one line.
</details>

<details><summary>Deleting a table</summary>

	DROP TABLE weather;
	
This will destroy the table in the previous example.
</details>

<details><summary>Entering new data</summary>

	weather (
	    city            varchar(80),
	    temp_lo         int,           -- low temperature
	    temp_hi         int,           -- high temperature
	    prcp            real,          -- precipitation
	    date            date
	);
	
Using this table again, if we want to insert new information, we can do it like so:

	INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
	
This will insert a row in the table that looks like so:

	city: 'San Francisco'   temp_lo: 46    temp_hi: 50    prcp: 0.25    date: '1994-11-27'

However, entering data like this means we have to remember the order of the columns in the table.  Explicitly name the data, and you don't have to remember the order:

	INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
    
Let's say it rained one day, but you forgot to measure prcp.  It's ok, you can just omit it:

	INSERT INTO weather (date, city, temp_hi, temp_lo)
    VALUES ('1994-11-29', 'Hayward', 54, 37);

</details>

<details><summary>Getting data</summary>
We'll use this table, again called weather, for our example:

	city      | temp_lo | temp_hi | prcp |    date
	---------------+---------+---------+------+------------
	 San Francisco |      46 |      50 | 0.25 | 1994-11-27
	 San Francisco |      43 |      57 |    0 | 1994-11-29
	 Hayward       |      37 |      54 |      | 1994-11-29
	 
Use SELECT to print out columns.

	SELECT city, temp_lo FROM weather;
	
Will print out:

		city        | temp_lo |
	---------------+---------+
	 San Francisco |      46 |
	 San Francisco |      43 |
	 Hayward       |      37 |
	 
Use * to specify all columns, and WHERE to specify rows.

	SELECT * FROM weather WHERE city = 'San Francisco'
	
Will print out:

	city      | temp_lo | temp_hi | prcp |    date
	---------------+---------+---------+------+------------
	 San Francisco |      46 |      50 | 0.25 | 1994-11-27
	 San Francisco |      43 |      57 |    0 | 1994-11-29
	 
You can combine information in different ways as well.

	SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;
	
In this example, the AS keyword means that we take the temperature calculation and print it out in a new column called temp\_avg, as follows.

	     city      | temp_avg |    date
	---------------+----------+------------
	 San Francisco |       48 | 1994-11-27
	 San Francisco |       50 | 1994-11-29
	 Hayward       |       45 | 1994-11-29


</details>

<details><summary>Join Queries</summary>

	city      | temp_lo | temp_hi | prcp |    date
	---------------+---------+---------+------+------------
	 San Francisco |      46 |      50 | 0.25 | 1994-11-27
	 San Francisco |      43 |      57 |    0 | 1994-11-29
	 Hayward       |      37 |      54 |      | 1994-11-29
	 
	 	name      | location
	---------------+---------
	 San Francisco |   (-194, 53)
	 
Here we'll use the same weather table as before, and now we have a second table called cities.

Before, we only asked for data from one table at a time.  Now we're going to ask for data from two tables to be combined.  Let's say we wanted to get all the information we had on the city of San Francisco.  Our data is spread over multiple tables, like above.  We could get all of San Francisco's data in a single output as follows:

	SELECT * FROM weather, cities WHERE city = name;

Will print out:

	city      | temp_lo | temp_hi | prcp |    date    |     name      | location
	---------------+---------+---------+------+------------+---------------+-----------
	 San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-194,53)
	 San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-194,53)

So now we have the temperatures, prcp, date, and location all in one place.  Notice that the city and name column are the same.  We could get rid of the redundant name column with this query:

	SELECT city, temp_lo, temp_hi, prcp, date, location
	    FROM weather, cities
	    WHERE city = name;

</details>

<details><summary>Join Qualifiers</summary>

	city      | temp_lo | temp_hi | prcp |    date
	---------------+---------+---------+------+------------
	 San Francisco |      46 |      50 | 0.25 | 1994-11-27
	 San Francisco |      43 |      57 |    0 | 1994-11-29
	 Hayward       |      37 |      54 |      | 1994-11-29
	 
	 	city      | location
	---------------+---------
	 San Francisco |   (-194, 53)
	 
Here are the 2 tables from before, with 1 difference:  the name column of the cities table is now called city.  Now both tables have a column called city.  How do you join on them?  Qualifying just means appending the table name to the column name so that Postgres can tell the difference.  Here's an example:

	SELECT weather.city, temp_lo, temp_hi,
	       prcp, date, location
	    FROM weather, cities
	    WHERE cities.city = weather.city;
	    
Here, weather.city and cities.city are the qualified terms.  Nothing else has to be qualified (though supposedly it's good style) since the other names aren't shared between the tables.
	    
	    
</details>

<details><summary>Agregate Functions</summary>

Aggregate functions let you get a single number from an entire column or row.  Things like max, sum, and avg.

<details><summary>In the weather table, how would you get the highest temperature overall?</summary>

	SELECT max(temp_hi) FROM weather;

</details>

<details><summary>What about the city corresponding to the highest temperature?</summary>
Incorrect:

	SELECT city FROM weather WHERE temp_hi = max(temp_hi);
	
This won't work because WHERE decides what rows to include, and WHERE is also calculated before and agregate functions, such as max.  In order for max to happen before WHERE, we do this:

	SELECT city FROM weather WHERE temp_hi = (SELECT max(temp_hi) FROM weather);
	
With the parenthesis in place, first we get the maximum temperature.  At this point, we don't know the corresponding city.  Now that we have the highest temperature, we then look through the whole table again to see which city has a temperature identical to this.

</details>


</details>

<details><summary>Views</summary>

If you have a big table, and you find yourself making the same query over and over, you can turn it into a view of the table.  This just saves the query in a variable.  You could just make another table that contains only the information you want to see, but this takes up additional space.  Using a view means doing the query over and over again, which is less time efficient than making another table, but since most queries are pretty much instantaneous to users, time efficiency is not a concern.  However, if you have a bunch of really similar tables that you created from doing a bunch of queries, the space that those tables take up can increase really fast.

Here's an example using our weather and cities tables:

	CREATE VIEW myview AS
	    SELECT city, temp_lo, temp_hi, prcp, date, location
	        FROM weather, cities
	        WHERE city = name;
	
	SELECT * FROM myview;

</details>

<details><summary>Foreign Keys</summary>
Say you have the weather table, and cities table.  You want to make sure users can only add city data to the weather table if that city is already in the city table.  You can do this by first looking at every entry in the name/city column of the cities table and doing a comparison.  However, Postgres offers an easy solution:

	CREATE TABLE cities (
	        city     varchar(80) primary key,
	        location point
	);
	
	CREATE TABLE weather (
	        city      varchar(80) references cities(city),
	        temp_lo   int,
	        temp_hi   int,
	        prcp      real,
	        date      date
	);
	
So now city in the weather table will look in the cities table every time you try to insert new data into the weather table.  So whenever someone tries to insert a new city, say 'Berkeley', it will error out, as 'Berkeley' is a foreign key to the cities table, which only contains San Francisco and Hayward.

</details>

<details><summary>Transactions</summary>
Remember atomicity from operating systems?  This is just that.  If we have a certain set of actions that we want to happen all or nothing, we label it as a transaction.  Consider the following example, where Alice gives Bob $100:

	UPDATE accounts SET balance = balance - 100.00
	    WHERE name = 'Alice';
	UPDATE branches SET balance = balance - 100.00
	    WHERE name = (SELECT branch_name FROM accounts WHERE name = 'Alice');
	UPDATE accounts SET balance = balance + 100.00
	    WHERE name = 'Bob';
	UPDATE branches SET balance = balance + 100.00
	    WHERE name = (SELECT branch_name FROM accounts WHERE name = 'Bob');
	    
	    
It would be really bad if Alice lost 100, then the power got cut, and Bob didn't receive 100.  Or if Bob got 100, and Alice didn't lose 100.  Here's how to make the series of operations atomic:

	BEGIN;
	-- Insert transaction between Alice and Bob here
	COMMIT;
	
Exactly how this works under the hood is covered in the concept of atomicity in Operating Systems.

By default, all Postgres statements get wrapped with a BEGIN and COMMIT.

</details>

</details>

`Shift Reduce Parsing`

There are a few techniques for parsing, but we're only going to learn about shift reduce parsing since that's the kind of parser that Bison generates.

SR parsers look through the tokens 1 at a time.  An SR parser keeps a stack of `items`.  Each item either a terminal (token) or non-terminal.  At each step of parsing, we do 1 of 2 things:  

1 We could take a token from the token stream and put it on our item stack.  This is called a `shift`.

2 We could use a production rule to combine multiple stack items into a single item.  This is a `reduce`.  Every time we reduce, we add a new node to our parse tree.

An SR parser shifts and reduces over and over until it can't shift or reduce anymore.  If there is no more input, and the only item on our stack is the start symbol, we have successfully created a parse tree.

Exactly how the SR parser decides whether to shift or reduce will remain mysterious for now.  Just know that sometimes the parser could either shift or reduce, and it always chooses the 'right' one to do.

Below is an SR parsing example.  In our example grammar, T and E are non-terminals, and int, *, and + are terminals (tokens).  Our example input to the parser is 'int * int + int'.  The vertical bar character | is used show where we are in our parse.  For instance, 'int * int | + int' means that our parser has looked at the 3 tokens 'int * int' and has put them on the item stack.  The parser has not yet looked at '+ int', and it has not yet reduced anything.  Whenever a shift move occurs, we put another token onto our stack, and move the | over by 1.  When we do a reduce move, some things to the left of the | will be changed, and we won't look at another token of input for that step.

def slideshow(slideNum):
    return SVG(filename='./SR-Parser/SR-Parser-Page-' + str(slideNum) + '.svg')

wg.interact(slideshow, slideNum=wg.IntSlider(min=1,max=11,step=1));

<details>
<summary>
Reductions seem backwards.  If Production 1:  T -> int has the arrow going from T to int, why are we turning ints into Ts?
</summary>

To answer this question, I have to explain some history.  The first parsers did not use the shift reduce algorithm.  They instead used `recursive descent`.  Rather than taking tokens and combining them into non-terminals, they started at the start symbol non-terminal, and broke up the start symbol into its child terminals and non-terminals.  For example, it would start at E, and break that up into T using production 4.  Sometimes (most of the time) it would choose the wrong production.  So it would have to undo all of its work and break E up into T + E using production 5.

Since recursive descent parsers came first, backus nar form has the arrows going from non-terminals to the parts that you break it up into.

Recursive descent parsing is slow.  It can be sped up by using special kinds of CFGs, called LL(k) grammars.  If you use an LL(k) grammar, recursive descent is as fast as shift reduce.  However, LL(k) grammars are annoying to create.

Interestingly, GCC and Clang (the two most popular C/C++ compilers) both use hand-coded recursive descent compilers.  They don't use bison, and they don't use shift reduce.  Supposedly it's easier to maintain your code if you do everything by hand.  But there are other languages like Ruby that do use parser generators like Bison.  Right now, I would recommend using a parser generator like Bison.  It's much less work.  C/C++ have to use hand-written because they're bloated from years of committees of people piling on features that were always conflicting with eachother.  It's a giant mess. 
</details>

<details><summary>Why does the parse tree end with a bunch of non-terminal symbols on it?  I thought it was supposed to only have terminal symbols.</summary>
You are correct.  After we create the parse tree, we will remove all of the nonterminal symbols.  Exactly how we do this will come after we learn how the parser decides whether to shift or reduce. 
</details>


So how does the parser know which production to use?  Well, you know to use a production if that production can be traced back to the start symbol.

The whole parser is a giant graph of shifts/reduces that can be traced back to the start symbol.  Here's a parser that can parse our E, T grammar:

<img src="./ShiftReduceStepByStep/ShiftReduceStepByStep-Page-35.svg" />

Here's how the graph works:

Remember that our parser works in steps.  At each step, the parser decides whether to shift or reduce.  It decides by looking through the stack, bottom to top.  At each item in the stack, it makes a transition in the graph.

Before explaining each part of the graph, let's first go through an example.

Let's say we're parsing `int * int + int`, and the parse stack is `int * | int + int`, so 'int' and '*' are on the stack.  Now the parser decides what's next:  shift the next 'int', or reduce something on the stack?

The parser looks at its graph, always starting in the start state.  Then it looks at the bottom of the stack, sees an 'int', and travels to state 4.  Then it sees a '*' and travels to state 8.  Now that the parser has gone through the entire stack, the final state is 8, and the shift/reduce decision is based on state 8.

Each state has a bunch of `items` in it.  An item looks like a production with a . somewhere on its right hand side.  An item says 'This is a potential production that we could reduce by.  This dot tells us how much of the production we have seen so far.'  

So in the start state, the item S\` -> .E says:  We haven't seen anything yet, because there's nothing to the left of the '.'.  If the next item on the stack is an E, we can transform that E into an S\`.

An item says to shift if there's at least 1 thing to the right of its '.'.  If there is nothing to the right of the '.', then the item says to reduce.

If the parser ended in state 10, it would decide to shift.  If the parser ended in state 11, it would decide to reduce by the production `T->(E)`.

But most of these states have multiple items; how do we decide for those states?  First, notice that all the items are shift items for most of these states, like State 1, State 5, and State 6.  They all tell you to shift, and unlike reducing, there's only 1 way to shift an item.

Here's how we actually made this graph:

	
    
def slideshow(slideNum):
    return SVG(filename='./ShiftReduceStepByStep/ShiftReduceStepByStep-Page-' + str(slideNum) + '.svg')

wg.interact(slideshow, slideNum=wg.IntSlider(min=1,max=35,step=1));

However, there is a problem.  States 3 and 4 both have 1 shift item and 1 reduce item.  This is a `shift/reduce conflict`.  As our graph is now, the parser can't decide whether to shift or reduce.  We can alleviate this problem by using follow sets, which I don't feel like explaining properly.  Basically, we just peek at the next thing on the stack without putting it on the stack.  If the next thing in the input is a '+', we shift.  If it's not a '+', we reduce.  There are also `reduce/reduce conflicts` which are also solved with follow sets.

There's also another problem:  the runtime of this algorithm.  Our parser starts at the start state at every shift/reduce decision.  We can improve this by putting things on the stack along with the state that we finished on.  So instead of `int * | int + int`, we would have `(int 4) (* 8) | int + int`.  The things on the stack say 'we shifted an int because state 4 told us to, and \* because state 8 told us to'.  Now at the next shift/reduce decision, our parser looks at the thing at the top of the stack, (* 8), and starts in state 8.



<details><summary>How do you tell where shift/reduce and reduce/reduce conflicts are just by looking at some productions?  How do you tell when a production is ambiguous?  What is the difference between a shift|reduce/reduce conflict and an ambiguity?</summary>

Conflicts are a specific kind of easily resolvable ambiguity.  A conflict is where the compiler says 'I have these 2 paths and don't know which one to take' and you explicitly tell it 'just take this one'.  This 'resolves' the ambiguity without actually making the grammar unambiguous.  It's a cheat rule, a patch, however you want to think of it.

Shift reduce conflicts are things like dangling elses.  They're pretty normal, and easy to resolve.  Reduce/reduce conflicts mean there is probably something wrong with your grammar.  Here's a reduce/reduce conflict in Bison:

sequence:
  %empty         { printf ("empty sequence\n"); }
| maybeword
| sequence word  { printf ("added word %s\n", $2); }
;

maybeword:
  %empty    { printf ("empty maybeword\n"); }
| word      { printf ("single word %s\n", $1); }
;

If you get a word, do you reduce by maybeword's production 2, or sequence's production 3?  A reduce/reduce conflict means that there is some redundancy in your grammar.


</details>

If doing the whole follow set thing doesn't work, then your grammar isn't `SLR`.  Also LALR.  Also ambiguity.
If you make sure every element in the follow set of every thing corresponds to a unique production, you might not have to worry about LALR, since I think having this 'unique element' thing would make your grammar SLR.

You also need to explain how this gets turned into that really complicated table.

Also instead of S\` you should just have S, which goes to E followed by dollar sign (end of input).  I don't know why Aiken wasn't explicit on that part.

Need to explain how epsilon productions work in your DFA:  remember that its an epsilon production, not an epsilon transition.  So you know to make the transitions based on what is immediately to the right of the '.'.  But For epsilon transitions, you make transitions for everything in their follow set.