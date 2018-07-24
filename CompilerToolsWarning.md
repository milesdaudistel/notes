You keep going back and forth on your ideas.  What tools should you use?  Should we change how we think about grammars?  Why is implementing a language so difficult?

The real solution to all of this debate over whether to use parser generators or hand write them or what parser generator is best is this:  keep your grammars simple.  If you don't implement over designed, masturbatory features then you won't have to jump through hoops to code your language.  I leave this document as a warning to myself.

Just use flex and bison.  Antlr is LL, and requires you to have weird, stupid looking grammars.

I have a radical new idea:  explain everything context-sensitive, going downward.  Maybe start with earley's algorithm or something.  Then explain that context-free is faster.  It would put all of these edge cases into perspective.  You could explain that 'you could either use tricks like precedence rules, or you could change the grammar.  Everything that isn't changing precedence rules is a hack'.  It would also help you figure out the best way to structure things.  It's also a more 'top down' learning approach.  As you've said before, the program should conform to the grammar, not the other way around.  If you start from context-free and try to build your way up to an actual programming language using hackery, you'll fuck yourself.  Would also let you explain the differences between LR, LALR, SLR, etc.  After you finally finish week 4.  Context-free isn't even difficult.  It's just backus nar but there's more than 1 thing on the left hand side.  Just explain how it works, and how you do the algorithm.  Then show how the grammars can be narrowed to be faster.  Show that it works on a per-rule basis, not just as a whole.  Maybe get started with the 'dangling else' thing.  Resolving ambiguities.
https://softwareengineering.stackexchange.com/questions/338665/when-to-use-a-parser-combinator-when-to-use-a-parser-generator/338888
This person seems very knowledgable.
Oh, also know what parts of your grammar thing can be automated.  Like I think left factoring can be automated.

context sensitive grammars

backus nar form

what grammars do in general

examples of how every language is context sensitive

dangling else and other specific things.  maybe look at wikipedia for all the common context sensitive / ambiguity.  Difference between ambiguity and all the other stuff.  Are C++/Java ambiguous?

recursive descent / earley's / whatever algorithm

runtime of context sensitive grammar

conversion to context free grammar and hacks to also make it O(n).

all the different context free grammar types.  LR, LALR, SLR.  LL(*).

Perhaps an example of scannerless parser.  Why it might be good or bad.  More or less just to show why scanners are necessary.

    for i in x:
      for j in y:
        print j
        print i
      print
      
If you get to the last print, you must choose whether to shift or reduce.  
You must choose to reduce the for.... print i to a FOR nonterminal.
Oh actually wait I think it's because the lexer has to count the whitespace.  This is context sensitive.  You have to keep track of some state.  Once it's turned into tokens, it's context free.  but if you consider it just of characters, it's context sensitive.

if CONDITION
tab EXPRESSION newline
tab EXPRESSION newline
else newline
tab EXPRESSION newline
 
IF_BLOCK -> "if" CONDITION STATEMENT_LIST ELSE_BLOCK_OPTIONAL

CONDITION -> lots of stuff

STATEMENT_LIST -> STATEMENT | STATEMENT STATEMENT_LIST

STATEMENT -> tab EXPRESSION newline

EXPRESSION -> lots of stuff

ELSE_BLOCK_OPTIONAL -> "else newline" STATEMENT_LIST | epsilon



if CONDITION
tab EXPRESSION newline
tab if  CONDITION
tab tab EXPRESSION
else newline
tab EXPRESSION newline















