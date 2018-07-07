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