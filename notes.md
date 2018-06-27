#Markdown
`#` These are used to make headers.  The more you have, the smaller the header.

>
One #:  
#Header
Two ##:
##Header
Three ###:
###Header
>

`>` Surround something in these to make a block like above.

`Backtick` Use this to put things in a box.  Technically this is a code block.

***
#Code Structures
`Interface` This is a really overloaded term.  It doesn't have an exact definition, but is a bunch of concepts in different languages that are nearly identical.

In general, an interface is a bunch of method names you attach to a class.  Think of it like different types of cars.  Under the hood, different cars operate very differently, but if you know how to use the steering wheel, break, and gearbox, then you can drive just about any car without needing to know exactly how it works.

TODO make this actual java syntax.  Also change it to a vehicle example.

TODO figure out how to collapse these examples.

Java Example

```
Interface Animal {
	void eatFood(int food);
	int poop();
}

Class Cow implements Animal {
	int foodLevel = 0;
	void eatFood(int food) {
		foodLevel += food
	}
	int poop() {
		System.out.println(foodLevel);
		foodLevel = 0;
	}
}

Class Cat implements Animal {
	int foodLevel = 0;
	void eatFood(int food) {
		foodLevel += food*2
	}
	int poop() {
		if (foodLevel > 0) {
			System.out.println(1);
			foodLevel--;
		} else {
			System.out.println(0);
		}
	}
}

Animal[] animals = new Animal[4];
/* put cats and cows in the array.  Hidden on purpose.  */
for (animal : animals) {
	animal.eat(1);
}

TODO change this into a borrowCar method where you borrow a random car?
```
In this example, the array has both cats and cows in it.  You don't know whether each object in the array is a cat or a cow, but it doesn't matter, because you know either way you can call eatFood or poop.  

You could make something equivalent to an interface in C++ by just making an abstract class with purely virtual methods.
TODO make an example here as well.

Interfaces in Go.

#Operating Systems
Why do Linux users always use thinkpads?  Canonical and Red Hat certify which laptops can run Linux.  Pretty much all Thinkpads are certified.  If you have a laptop that is not certified to run Linux, there might not be a sound driver, or a wifi driver, and you're wrecked.  TODO insert picture of Thinkpad.

Why are there so many flavors of Linux?  What's the difference between them?  Some kinds of Linux adhere to the free software spirit, which people like.  Others are pretty.  If you want to know whether switching from MacOS to Linux will make you a better programmer, it won't.  However, switching from Windows might be a better experience.  Empirically, I've found Unix based systems to be easier to develop on, as most common development software like GCC and Clang work out of the box on those.  However, Windows would require Cygwin in order to run that software.  Installing more software is never fun.
