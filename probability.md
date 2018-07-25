#Probability
`counting` as in counting cards, is the most basic kind of probability.  The kind of questions that counting answers are like this:  You have a set S, and you pick elements from that set for your new set T. How many different T's could you make?  

There are 2 questions you should ask that determine how you solve this problem:

Does `order` matter? Is T = {x, y} the same as T = {y, x}?  If the answer is yes, then order does not matter.  If the answer is no, then order matters.

Is there `replacement`?  Lets say S = {x, y, z} and you want to choose 2 elements for T.  If you choose x, does S = {y, z} now, or is it still S = {x, y, z}?  If x is gone from S, then there is no replacement.  If x is still in S, then there is replacement.  If there is replacement, then you could have T = {x, x}.  If there is no replacement, then it would be impossible for you to get T = {x, x}.

Say you have 100 pokemon cards spread out in front of you, where each card is unique.  You want to assemble a deck of 10 of them.  How many possible decks could you have?  Note that if your deck is 

	pikachu jigglypuff charmander squirter bulbasaur
	
that's different from

	jigglypuff charmander squirter bulbasaur pikachu

First, lets say you pick up 1 of them.  How many possible decks could you have?  The answer is obviously 100.  Now, you pick up a second card.  How many possible decks now?  Well, your deck of 1 card could be 100 different things.  Now you

for 'with replacement' we'll say that you're photocopying the cards like a cheapskate.  Or maybe it's your cool uncle who has 'like infinity' of each pokemon card.