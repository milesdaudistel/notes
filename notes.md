#Financial Wizardry
You need money, right? Quick advice: invest in ETFs, put money into your 401k, then your roth ira. Below is a chart from reddit. It's a little small, maybe open it separately.

<details><summary>Personal Finances Chart</summary>
![financechart](pics/financechart.jpg)
</details>

#Code Structures



<details><summary>Interface</summary>

Languages like Java and Go, and C++ all have very similar but not identical concepts called interfaces.

In general, an interface is a group of unimplemented method names.  Metaphorically, if you think of different classes as different types of cars, then an interface for those all of those classes might be a steering wheel, gas pedal, break pedal, and transmission.  Because all of these different types of cars have the same interface, you know how to drive any of them without having to understand how they work under the hood.  So if you know an interface, you know how to manipulate every class that implements that interface without knowing how it works under the hood.

<details><summary>Pseudo Java Example</summary>

	Interface Vehicle {
		void takeGas(int amount);
		int milesPerGallon();
		void go();
	}
	
	Class Car implements Vehicle {
		int gasLevel = 0;
		int milesPerGallon = 25;
		void takeGas(int amount) {
			gasLevel += amount;
		}
		int milesPerGallon() {
			return milesPerGallon;
		}
		void go() {
			System.out.println("Went " + str(milesPerGallon * gasLevel) + " miles");
			milesPerGallon = 0;
		}
	}
	
	Class Motorcycle implements Vehicle {
		int gasLevel = 0;
		int milesPerGallon = 50;
		void takeGas(int amount) {
			gasLevel += amount;
		}
		int milesPerGallon() {
			return milesPerGallon;
		}
		void go() {
			System.out.println("Went " + str(milesPerGallon * gasLevel) + " miles");
			milesPerGallon = 0;
		}
	}
	Vehicle[] vehicles = new Vehicle[4];
	
	/* put cars and motorcycles in the array.  Hidden on purpose.  */
	
	for (vehicle : vehicles) {
		vehicle.takeGas(10);
		vehicle.go();
	}


In this example, we don't know whether each vehicle in the array is a car or motorcycle, but it doesn't matter, because either way you know that you can call takeGas, milesPerGallon, and go.  Note that you cannot create a Vehicle object.  You must create a class that implements the Vehicle interface.  In this way, a vehicle is like something like an abstract class. 
</details>

Interfaces aren't exactly the same in all languages.  In C++, there is no keyword for interface.  However, you could make something equivalent to a Java interface in C++ by creating an abstract class with purely virtual methods.  Then any class that inherits from this abstract class must implement these virtual methods.


<details>
<summary>Go example</summary>
In Go, interfaces are explicitly declared and implicitly implemented.  This means you create an interface by saying `type myInterface interface {...}` but unlike Java, there is no need to say `class X implements myInterface` in order for X to be able to use myInterface.

	package main
	import "fmt"
	import "math"
	
	type geometry interface {
	    area() float64
	    perim() float64
	}
	
	type rect struct {
	    width, height float64
	}
	type circle struct {
	    radius float64
	}
	
	func (r rect) area() float64 {
	    return r.width * r.height
	}
	func (r rect) perim() float64 {
	    return 2*r.width + 2*r.height
	}
	
	func (c circle) area() float64 {
	    return math.Pi * c.radius * c.radius
	}
	func (c circle) perim() float64 {
	    return 2 * math.Pi * c.radius
	}
	
	func measure(g geometry) {
	    fmt.Println(g)
	    fmt.Println(g.area())
	    fmt.Println(g.perim())
	}
	func main() {
	    r := rect{width: 3, height: 4}
	    c := circle{radius: 5}
	
	    measure(r)
	    measure(c)
	}
	
Here is an interface called geometry.  Both the rect and circle structs implement it without the need to say 'implements geometry'.  Go knows that rect and circle implement the geometry interface simply because both structs have an area and perim method.
</details>

<details><summary>Why does Java have interfaces and abstract classes?  Why not just use abstract classes?</summary>

</details>

</details>

<details><summary>Why use getters and setters?</summary>
	
	class Doggo:
		int bork = 4
		
Lets say you have this doggo class, and you don't want people to be able to change your bork.

	class Doggo:
		private int bork = 4
		
Cool.  But now no one knows about your bork but you.  You want people to be able to see your bork, but not change it.  You can do that with a public getter.

	class Doggo:
		private int bork = 4
		
		public Get():
			return bork
			
Now everyone can use Get to see a copy of your bork.  They will be able to change the copy of their bork, but they won't be able to change yours.  So that's getters.

What about setters?  Lets say you have some food, and it's ok to share it with people.

	class Doggo:
		private int bork = 4
		private int fud = 2
		
		public int Get():
			return bork
		
		public int Set():
		

	Doggo gb = Doggo()
	
//TODO provide an example that shows that it's more mutable.  People calling the code won't have to go back and change it.
 

</details>

<details><summary>Threads</summary>

Think of threads as jobs, not as workers.  It's important that you can spawn as many threads, or jobs as you want.  Your operating system has to output video, audio, listen for mouse clicks, keyboard input, watch power consumption, etc.  That's a lot more than 4 jobs.  If you could only run 4 jobs at a time the user would think their computer was super slow.  Well, not super slow, it just wouldn't function at all.

Another way to think about it is that your computer is your stay-at-home mom.  She has 4 tasks on hand: doing laundry, washing dishes, making breakfast, and vacuuming.  If your mom wanted to do the tasks in the order that saved the most time, she should probably do the laundry and the dishes before making breakfast or vacuuming.  She can load the clothes / dishes into their respective machine, and they'll keep working while she cooks and cleans.  But lets say its your first day back to school, so you have to leave soon.  If she does the laundry or the dishes, you'll have to leave before breakfast is done.  Being a smart mom, she makes breakfast for everyone first, even though it will take more time overall.

It's the same principle for your computers scheduler.  If you want to listen to music while reading a pdf, it would be most computationally efficient for your computer to play the entire song, then open the pdf, then let you scroll through it.  But that's obviously not what you want, so your computer switches between these two tasks (threads) very rapidly in order to make it seem like both things are happening at the same time.

</details>

<details><summary>Events, Asynchronous Methods, Callbacks</summary>

If a method is asynchronous, calling it will spawn another thread or process.  Imagine a user clicks on a desktop icon.  The method for opening up the app corresponding to the icon should be asynchronous, because if it wasn't, the user couldn't move their mouse after clicking the icon.  Asynchronous calls are important even if you only have 1 core.  Think of a thread as a job, and a core as a worker.  If you only have 1 worker, you can't have them work on just 1 job at a time.  That would be bad for the user.  So our 1 worker juggles multiple jobs at the same time, and users have the illusion that many operations are happening simultaneously.  

An event is just 'something that has happened'; a mouse click, a keyboard input, a new email, etc.  Since these things happen all the time, we need asynchronous methods that can run concurrently in order to catch and handle all these events.

But we don't just want async methods to catch events, we also want to tell them what to do with the events.  We can do this by passing the async method a callback function as a parameter.  An example is when you set onClick in an html tag.  The thing that you set onClick to is the callback function.  

You might wonder 'Why do we need to pass a callback in as a parameter?  Why not just put that code directly into the asynchronous function?'  To answer that, think about this scenario:  you have a website that has a whole bunch of different clickable buttons.  Some buttons link you to other pages, some buttons open drop down menus, some display images.  All of these buttons have asynchronous functions behind them, waiting for clicks.  But when they get a click, they all do different things.  If you had to program all these buttons, it would be much easier to write the part of the program that listens for clicks once, rather than copying and pasting it again and again for every single button.  So the reason callback functions are so common is that asynchronous functions often just listen for events, so passing in the 'what to do after you get the event' logic as a parameter is easier than writing it directly into the asynchronous method.

</details>

`polymorphism` Another somewhat loaded term.  I would say it usually refers to function overloading.  Consider the following 2 functions:

	int Add(int x, int y) {
		return x + y;
	}
	
	int Add(int[] x, int[] y) {
		return sum(x) + sum(y);
	}
	
Notice they have the same name, the same return type, but different parameters.  Some languages allow this, as they differentiate function calls by looking at the parameters.  Other languages don't bother doing this, and so don't allow this kind of method naming.

`first class function` If a programming language supports first class functions, this means you can pass functions as parameters, return them from other functions, etc.  Basically means you treat functions as data, like any other variable.

`Higher-order function` Also called functor.  A higher order function is a function that takes functions as arguments, returns a function, or both.

`closure` related to first class and higher-order functions, a closure is a function returned from a higher-order function (its parent) that can still access its parents variables.  Example:

	def makeHuman():
		noise = "oh no"
		def human():
			print(noise)
		return human
		
	x = makeHuman()
	x() #will print "oh no"
	
In this example, x assigned the human function.  When x is called, it knows the value of the 'noise' variable, even though it is outside the scope of human.  Of course, calling 'print("oh no")' would achieve the same thing.  The real power of closures is that they can do many of the same things that classes/objects can do:

	def makeMathematician():
		favoriteNumber = 4
		def getNum():
			return favoriteNumber
		def setNum(num):
			favoriteNumber = num
		return getNum, setNum
		
	x = makeMathematician()
	print(x.getNum()) #will print 4
	x.setNum(8)
	print(x.getNum()) #will now print 8

<details><summary>If closures and classes are so similar, which one should I use?</summary>
Classes and objects are much faster than closures.  I'm not really sure why.  I would say use classes when you can.  The mathematician example would actually be better represented as a class, I think.  For some languages like javascript, the only way to get classes is through closures.  So for now, I would treat closures as something to be dealt with, not something to actively try to use.
</details>

`Design patterns` aren't a library or a concrete feature of a language.  They're a way to structure code.  They're the vague kind of techniques that differentiate 'good' from 'bad' code.  If used correctly, they shoud make code more clear, more reusable, and more extendable.  If you're about to dive in to a large code base, it might help to google <type of software> design patterns.  Security design patterns, simulation design patterns, etc.  Probably the most important thing you can take away from design patterns is that they're not something you set out to use.  They're a result of the fact that most software does very similar things, and end up implemented in similar ways.  So keep them in mind, but remember that the term 'overdesigned' is used a lot more often than 'underdesigned' in the software industry, at least in my experience.

There's a famous book called 'Design Patterns: Elements of Reusable Object-Oriented Software' where most of these common design patterns get their name.  I wouldn't read it.  However, if you look at the following list:

	Abstract Factory, Builder, Factory Method, Prototype, Singleton, Adapter, Bridge, Composite, Decorator, 
	Facade, Flyweight, Proxy, Chain of Responsibility, Command, Interpreter, Iterator, Mediator, Mememto, 
	Observer, State, Strategy, Template, Visitor
	
You might have seen some of these terms as part of a function name, class name, type, etc in some piece of code that you read.  These terms are the design patterns described in the book.  So if you come across a piece of code that you don't understand, and it has one of these words in the name, you can google 'design pattern X' to get a high level understanding of the piece of code you're looking at.  Most of these I think aren't useful, though.  Below are the ones that I do find useful:

`Design patterns: factory method` One of the simplest and most useful patterns.  If you're making an API that has a lot of different related objects, you use a factory method to simplify creation of those different objects for the user of the API.  Consider this example that does _not_ use a factory method:

	//API
	abstract class Vehicle:
		void printWheels():
			print(0)
			
	class Motorcycle inherits Vehicle:
		void printWheels():
			print(2)
			
	class Car inherits Vehicle:
		void printWheels():
			print(4)
	
	//User of API
	class Person:
		Vehicle myVehicle = null
		
		Vehicle buyVehicle(vType):
			if vType == 1:
				myVehicle = Motorcycle()
			elif vType == 2:
				myVehicle = Car()
			
Seems pretty simple.  But what if the makers of the API wanted to add new types?  Bikes, skateboards, jet skis, big rigs, etc.  For every new subclass you add to the API, the user of your API will have to add another elif to their buyVehicle function.  Instead, we should do that for them.  Here's the same API, but now with a factory method called manufacture:

	//API
	abstract class Vehicle:
		void printWheels():
			print(0)
			
	class Motorcycle inherits Vehicle:
		void printWheels():
			print(2)
			
	class Car inherits Vehicle:
		void printWheels():
			print(4)
			
	Vehicle manufacture(int vType):
		if vType == 1:
			return Motorcycle()
		elif vType == 2:
			return Car()
		else:
			return null
	
	//User of API
	class Person:
		Vehicle myVehicle = null
		
		Vehicle buyVehicle(vType):
			myVehicle = manufactor(vType)
			
So our API code gets more complicated, but it's easier for the user now.  There could be a million different vehicle types and the user wouldn't need to change their implementation.  And since there's hopefully going to be a lot more users of the API than creators of the API, this should save a lot of typing for most people.

`Design patterns: singleton` A singleton is a class that you should only ever need 1 object of.  For instance, if you are playing some game app, there's probably a class called game that contains everything else.  You don't want more than 1 game running at a time, so you would make that class a singleton.  It's pretty simple to implement.  Here's a java example:

	class Singleton:
	    static Singleton obj = new Singleton();
	 
	    private Singleton(): pass
	 
	    Singleton getInstance():
	    	return obj
	
The obj object is the 1 instance of our class.  Notice the constructor is private, so no one can create another obj.  1 obj is created on start up, and you can interact with that 1 obj by calling getInstance.

`design patterns:  MVC` stands for Model-View-Controller.  Technically all 3 of these are design patterns on their own, but they're usually lumped together.  This design pattern is used very commonly in web development (which is basically the wild west of software; always changing, basically no rules.  At least no rules that stick around for more than a few years), so I won't write too much about this.  The model is the data.  User data, embedded videos, ability to comment on things, etc.  It's all the functionality of the website.  The view is the user interface.  It changes itself when it sees a change in the model.  The controller handles events.  It watches the user (and other possible events) and changes the model accordingly.  So the controller watches the user, the user watches the view, the user interacts with the view the controller changes the model accordingly, the view watches the model, the view changes itself accordingly.

Right now, you could say that 'backend' code like Go or backbone.js make the model, react.js is for making the view, and node.js is for making controllers.  But that's probably going to change once again a few years from now.

`design patterns:  template method` Basically just a very generic higher order function, or abstract class.  Implement the parts that are common between all the sub-parts, then leave the other parts unimplemented for someone else to implement.  Or provide a default anyway and let other people overwrite it.

![endsin1](pics/stolenfromsourcemaking.png)

Here, worker is the template method, whose work submethod gets replaced for each different worker.  The term `hook` is often used for the parts of the template that the user fills in.

`Framework` is just a bunch of template methods put together.  A framework is constricting to the user, offering them less flexibility, in return for making implementation a lot less of a hassle.  The term framework is usually used in web / enterprise programming.  I wouldn't say a single library is a framework because a library is a tool for a kind of _computational_ task.  A framework is for ... I'll say product-creation tasks for now.  Web framework, graphical framework, etc.

`Pagination` Lets say you have some application, and a user makes a really big request, for 1000 pictures, let's say.  The apps UI would take forever to render 1000 pictures all at once.  You could either 

a) go through with the request and make the user wait a few minutes

b) not give the user what they explicitly asked for

c) paginate the request, giving them a little bit of what they asked for (say a page of 10 pictures), to which the user could then request the next 10 pages, then the next 10, etc.

Guess which one is best?

#Databases

<details><summary>Table</summary>
A table stores data in rows and columns.  Each column is a type of data, and each row is a collection of that data.  Columns might be things like names, ages, jobs, etc.  Put these columns together and you have a table, where each row will give you the name, age, and job of a single person.
</details>

<details><summary>Database</summary>
A database is just a bunch of tables put together.  Maybe you have a table containing information about neighborhoods.  There could be a column for street names, house addresses, and the people who live at each house.  Each of those people corresponds to a row in the previous example.
</details>

<details><summary>SQL</summary>
SQL stands for Structured Query Language.  It's a language purely for doing things with data in a database.
</details>

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

#Operating Systems
<details>
<summary>Why do Linux users always use thinkpads?</summary>
Canonical and Red Hat certify which laptops can run Linux.  Pretty much all Thinkpads are certified.  If you have a laptop that is not certified to run Linux, there might not be a sound driver, or a wifi driver, and you're wrecked.
</details>

<details>
<summary>Why are there so many flavors of Linux?</summary>
Some kinds of Linux adhere to the free software spirit, which people like.  Others have a pretty desktop environment.  If you want to know whether switching from MacOS to Linux will make you a better programmer, it won't.  However, switching from Windows might be a better experience.  Empirically, I've found Unix based systems to be easier to develop on, as most common development software like GCC and Clang work out of the box on those.  However, Windows would require Cygwin in order to run that software.  Installing more software is never fun.
</details>

<details>
<summary>Virtual Machine</summary>
Your computer has an operating system, probably either Windows, MacOS, or Linux.  Within your operating system, you can use software like virtualbox to run another, different operating system inside of your current one.  So you can run Linux in Windows, Windows in MacOS, etc.  Useful if you need some functionality of both operating systems.
</details>

<details>
<summary>Container</summary>
A container is like a virtual machine, but lighter weight.  While a VM requires you to have a whole copy of an operating system, a container only requires you to have a copy of the parts of an operating system that you want to change.  Let's say you are running Linux, and you want to be able to run both python2 and python3.  Those might conflict with each other, so what you can do is create a container image for both of them.  The container image will have a copy of your .bash_profile (your PATH variable) and a different set of files in /lib/python.  With virtual environments, you would have to have Linux running on your computer, and 2 more images of Linux on top of that.  That means 3 copies of the kernel, 3 copies of GNU, 3 copies of every file on your computer.  That's a lot of memory, and it tends to be slow.  With containers, you start with just 1 copy of everything, then you make 2 more copies of your path variable, modify them, and 2 more copies of your python libraries, and modify them.  In a way, it's kind of like branching in a git repository, but for your operating system.  You only need to make a copy of things you are going to modify.

So, VMs are big and slow.  Containers are small and fast.  However, the more you modify in a container, the bigger it gets, and the more like a VM it becomes.

Of course, python virtual environments also solve this problem, but that's just for python.  Containers work for any piece of software you can think of.
</details>

<details>
<summary>Python Virtual Environment</summary>
Python VE's predate most container stuff that we know.  They're very similar, but outdated compared to containers.
</details>

`vagrant`
Allows you to make a VM, configure it, then share it with other people.  Requires virtualbox / vmware.  Used in businesses when you want everyone to run their code on the exact same system.  Solves the problem of 'but it runs on my machine'.  Why not just make a configuration in vmware / virtualbox, then share it with everyone?  Because then they can modify it (intentionally or not) which you don't want.  The whole point is to have everyone on the exact same system, and if they're able to modify their system, that defeats the purpose.

`Environment List`
When programs are compiled, linked, run, etc, how do they do various things like find standard libraries, know what shell it's being run in, who the current user is, etc?  Through the environment list, otherwise known as the program environment.  The environment list is passed to your program by whatever is running it (usually a shell), and is available globally.  Below is a diagram of a simpmlified environment list.  The real environment list contains dozens if not hundreds of name:value pairs.  Some other examples of environment variables might be SESSION=ubuntu, SSH\_AGENT\_PID=1508, etc.

![endsin1](pics/environmentlist.png)

`PATH variable`

To write Hello World in C, the first thing you do is `#include <stdio.h>`.  How does the compiler know where stdio.h is?  It looks for the PATH variable in the environment list.  The PATH variable specifies where to find all your standard libraries.  On MacOS, the PATH variable is usually contained in `~/.bash_profile`.

`Unix file system`

`/` is the root directory.  It contains all other files in your system.

`~` is your home directory.  It contains all your documents, pictures, etc.  Actual path varies, on Linux its `/home`, on MacOS it's `/Users/<yourname>`.

For the following subdirectories I am only guessing as to their function.  Most of the reason why there are so many directories is just because of history.  There is no more need for much of this complicated heirarchy with its names that have little to do with what the folder actually holds.

http://lists.busybox.net/pipermail/busybox/2010-December/074114.html

`/usr` used to be the home directory.  Now it contains shared resources that aren't 'system critical' like shared libraries.

`/usr/include` contains header files.  This contains C header files.  Some non-C stuff might be here, but pretty much all OS's are written in C, so odds are this folder won't have much other than standard C header files.

`/usr/lib` contains the source code for most libraries for most languages, like C, C++, Java, Python, Go.  This and /usr/bin are where your package manager installs things.

`/usr/bin` contains the compiled binary versions of the source files in /usr/lib.

`/usr/local` everything here is for your 'local' user.  None of it is managed by a package manager.  You might want this for something like, I don't know, game files, photoshop, stuff like that?  Just stuff you don't want your package manager updating and potentially breaking.

`systemd` This is the first process when you boot your OS.  It boots and monitors all userspace processes.

`systemctl`
ctl stands for control.  systemctl controls systemd, allowing you do do various things.
>`list-unit-files` lists all the services that systemd has available to run.

`journalctl`
journalctl -fu api -o json | jq.  What is this?

#Git
Git is version control software.  When you make a project, whether it's code or an essay or a painting, you start with nothing, and gradually make changes until you get a final product.  You add changes, you remove changes, you start over, etc.  Without version control, all you have is the current version of your project.  There's no way to see the history of your project; what it looked like yesterday, a week ago, whatever.  With git, you can do this.  You can also split a project into 2 different projects and track both of them at the same time.  

http://www.graphviz.org/

try this for your node examples.


`HEAD` is the current commit you're on.

`detached HEAD`
  If you check out a commit that isn't a leaf node in the git tree, 
what is a detached head

`remote`

`origin`

`add`

`rm`

`commit`

`clone`

`https vs ssh`

`merge` fuse 2 branches together.

do deletions get automerged?

what happens if you're behind, and you push without pulling first?

`rebase` snap off your branch and stick it on the end of another branch.

`tag`

`stash`

`squashing`



#Json

Json is a data format that is able to be read by humans as well as any programming langauge.  It's used to pass data between different languages.

<details><summary>JSON general format</summary>

	object
		{}
		{ members }
		
	members
		pair
		pair , members
		
	pair
		string : value
	array
		[]
		[ elements ]
	elements
		value 
		value , elements
	value
		string
		number
		object
		array
		true
		false
		null
	
	string
		""
		" chars "
		
	chars
		char
		char chars
		
	char
		any-Unicode-character-
	    except-"-or-\-or-
	    control-character
		\"
		\\
		\/
		\b
		\f
		\n
		\r
		\t
		\u four-hex-digits
		
	number
		int
		int frac
		int exp
		int frac exp
		
	int
		digit
		digit1-9 digits 
		- digit
		- digit1-9 digits
	
	frac
		. digits
		exp
		e digits
		
	digits
		digit
		digit digits
		
	e
		e
		e+
		e-
		E
		E+
		E-
</details>

<details><summary>General examples</summary>

	{ "yung arfy": {
		"legs": 4
		"bork": "arf arf"
		"good boy": true
		}
		
	  "ol borko": {
		"legs": 3
		"bork": "bOoOork"
		"good boy": true
		}
		
	  "precious": {
		"legs": 4
		"bork": "..."
		"good boy": true
		}
	}

	{"menu": {
	  "id": "file",
	  "value": "File",
	  "popup": {
	    "menuitem": [
	      {"value": "New", "onclick": "CreateNewDoc()"},
	      {"value": "Open", "onclick": "OpenDoc()"},
	      {"value": "Close", "onclick": "CloseDoc()"}
	    ]
	  }
	}}
	
	
</details>



#HTML
HTML stands for Hyper Text Markup Language.  All it does is display tet that you 'mark up' with tags and elements to change how it is displayed.  Since HTML's structure is just putting text between tags, it's very difficult to code anything substantial in HTML.  That's why we have javascript to embed complicated stuff into an HTML page.  For example, lets say wikipedia is written in pure HTML.  There's no way to log in to wikipedia, or play web games, etc.  In order to do those things, javascript is necessary, because it allows for more complex back and forth communication between the client and server.  In pure HTML over a TCP/IP connection, a client can only ask the server to send them different static web pages.  With javascript, the client can ask things like 'show me my account details' and javascript can communicate that back to the server.  Stuff like that.

<details><summary>tag</summary>
Tags are how you say how to format text in HTML.  They're the most basic thing.

	<b><i> Hello </b></i>
	
results in

<b><i> Hello </b></i>

Since b means bold, and i means italic.  This is all that HTML does.  It just formats text and pictures in a way that is able to be transmitted over the internet.
</details>

<details><summary>element</summary>
An element is just anything between two tags, the tags included.

	<b><i> Hello </b></i>
	
This whole thing is an element.
</details>

<details><summary>attribute</summary>
An attribute is something you add to a tag to specify more information.

	<img src="my_img.jpg" width="500" height="200" alt="couldn't find her">
	
This is an img element.  The tag is img and the attributes are src, width, etc.  Some attributes are required, like src, and others are optional, like width and height.  Also note that this is a `singleton` because it doesn't need a closing tag which would be something like 

	</img>

</details>


<details><summary>view source</summary>

The view source is the code that makes up a web page.  If you wrote the page in pure HTML, it's exactly what you wrote.  However, it might be different if you wrote a template file, embeded javascript, etc.  In reality, most web pages are not pure html, so what you get is a bunch of 

</details>

<details><summary>DOM</summary>
The Document Object Model is the api for interacting with and changing html.  Javascript doesn't actually change html directly, it does it through the DOM.

	<div id="container"></div>
	
Here is some random html.  It does not change by itself, or run, or do anything in particular.

	<script>
	  var container = document.getElementById("container");
	  container.innerHTML = "New Content!";
	</script>

This is a javascript method embedded in the same document as the above div element.  It calls the DOM through `document.getElementById` in order to change the above div tag to say "New Content!"

</details>

#Javascript
<details><summary>keywords</summary>
The `function` keyword can be used to define a function.

	function myFunction(p1, p2) {
	    return p1 * p2;
	}

It is also used to declare anonymous functions.

	var getRectArea = function(width, height) {
	    return width * height;
	}
	
Fat Arrows (`=>`) are a way to create anonymous functions.  Example:

	const z = (x, y) => { x * y };
	
Now z refers to an anonymous multiply function that takes in parameters x and y.  Note the function gives back x * y; the return keyword is omitted.  The last line of a fat arrow function is implicitly returned.  Another feature of fat arrows is that they don't provide a binding for the keyword this, and so this will retain its definition from the outer scope.
	
Unlike the rest of javascript, the 'this' keyword is dynamically scoped for some reason.  On a global scope, it refers to the global object (the window).  In a class, it refers to the object of the class that is calling the method.  But what if you pass this as an argument to another function outside of that class?  The clock class component in the react.js section is a good example of this.  

	class Clock extends React.Component {
	
	  /* omitted functions */
	  	
	  componentDidMount() {
	    this.timerID = setInterval(
	      this.tick,
	      1000
	    );
	  }
	  
	  /* more omitted functions, including tick function */
	  
	}

In this example, we pass the tick function of clock to setInterval, which is a function outside of clock.  Simply passing this.tick to setInterval will not work, because setInterval is a built in global method, so this will refer to the global object.  To fix this, we should instead pass `() => this.tick()` to setInterval, which is just a fat arrow function that passes in no parameters. 
	
</details>

<details><summary>important functions</summary>

`bind`
takes in a variable / function / class and outputs a 'bound' version of that thing.  A 'bound' variable / function / class changes the 'this' keyword to be lexically scoped for that specific function.

	var module = {
	  x: 42,
	  getX: function() {
	    return this.x;
	  }
	}
	
	var unboundGetX = module.getX;
	console.log(unboundGetX()); // The function gets invoked at the global scope
	// expected output: undefined
	
	var boundGetX = unboundGetX.bind(module);
	console.log(boundGetX());
	// expected output: 42

In general, I believe the way you bind a function is function.bind(scope that you want to bind function to).

</details>

<details><summary>Classes / Prototypes</summary>

javascript classes -> callbacks -> promises -> async functions -> react async states

javascript doc on prototypes has examples that dont work, so all of the below is garbage.  Don't bother reading it until you find out how inheritance actually works.

Javascript classes (here called prototypes) are looser than classes in C or Java.  A prototype creates objects just like classes, but a prototype's methods and fields can be modified, added to, or taken away from.  If a prototype gets modified, all of the children spawned from it get modified.  This includes other prototypes inheriting from it.

	function myclass
		bla bla(this)
		
	myclass.prototype = Object.create(myclass.prototype or some class that its inheriting from)
	
	var x = new myclass
	
myclass.prototype will get put into function myclass, which will then return an object and put it in x

best way to implement inheritance is through the call method

	function Worker(name, projs) {
		this.name = name || "";
		this.projs = projs || [];
	}
	
	function Engineer(name, projs, mach) {
	  Worker.call(this, name, projs);
	  this.machine = mach || '';
	}
	
	Worker.prototype.name = "Jerry";
	
	function Myclass(param1, param2) {
		this.param1 = param1 || '';
		this.param2 = param2 || '';
	}
	
	var x = new Myclass(<nothing>, "myparam2");
	
In this example, we have a Worker class, and an Engineer class that inherits from Worker through the call method.  If a parameter is not specified, its value is whatever is to the right of the ||.  After we create these 
	


</details>


#React.js
React is a javascript library for UI stuff, and only UI stuff.  Node.js is for making a server (the client/server paradigm).  Angular is for both making a server and making a UI.  

	const element = <h1>Hello, world!</h1>;
	
This is not javascript, and it is not html.  It's jsx, which can be read by react and translated into javascript.  It can do everything javascript can do.  Remember how javascript can be embedded in html?  You write html, then you write javascript, then you reference the js file in your html.  Markup(html) and logic(js) are separate.  In jsx, they are not separate, and that makes it easier to read and write UI stuff.



<details><summary>basic syntax</summary>
	
	function formatName(user) {
	  return user.firstName + ' ' + user.lastName;
	}
	
	const user = {
	  firstName: 'Harper',
	  lastName: 'Perez'
	};
	
	const element = (
	  <h1>
	    Hello, {formatName(user)}!
	  </h1>
	);
	
	ReactDOM.render(
	  element,
	  document.getElementById('root')
	);
	
jsx looks a lot like javascript.  Most of this stuff is just regular javascript.  The element part could have been written as `const element = <h1> Hello, {formatName(user)}!</h1>;` but it's a little big to be on one line.  If you want your html portion to be on multiple lines, put it in parenthesis.  Also note that formatName(user) is in curly braces.  This indicates that we should treat it like regular javascript.

Just like you can put javascript into html with jsx, you can put html into your javascript.

	function getGreeting(user) {
	  if (user) {
	    return <h1>Hello, {formatName(user)}!</h1>;
	  }
	  return <h1>Hello, Stranger.</h1>;
	}
	
This javascript-like function returns html stuff.

To specify a tag attribute, put it in quotes.

	const element = <div tabIndex="0"></div>;
	
Tag attributes can also be javascript stuff in curly braces.

	const element = <img src={user.avatarUrl}/>;

</details>

<details><summary>elements</summary>
jsx creates react elements, the basic building block of react, which get rendered to the DOM.  Elements describe what you want to see on the screen.

	const element = <h1>Hello, world</h1>;
	ReactDOM.render(element, document.getElementById('root'));

This element is turned into a DOM root node by the render function.  Specifying to react that an element is a root node means that the react-specific DOM will take care of the root, and all of its children.  For most commercial apps, you'll need a combination of react and non-react stuff, so a root node doesn't just mean 'everything in this file'.

React elements can't be updated, they are static.  In order to change a react web page, the element must be rerendered.
</details>

<details><summary>components</summary>
Components jsx wrappers to javascript functions.  They take in props (short for properties, same thing as parameters), and return react elements.  Props are strings, ints, elements, or other componenets.  Components cannot modify the value of their props.

Here's an example component, Welcome:

	function Welcome(props) {
	  return <h1>Hello, {props.name}</h1>;
	}
	
	const element = <Welcome name="Sara" />;
	ReactDOM.render(
	  element,
	  document.getElementById('root')
	);

Welcome takes in props, and returns a new element saying Hello, name.  Then const element is set to the return value of Welcome when passed the prop "Sara".

An important note is that components must start with an upper case letter.  If they don't, jsx will not recognize them as components.

Now we're going to look at why components are useful.  They can encapsulate more than a simple jsx function is capable of.  Consider the following example of a clock function:

	function Clock(props) {
	  return (
	    <div>
	      <h1>Hello, world!</h1>
	      <h2>It is {props.date.toLocaleTimeString()}.</h2>
	    </div>
	  );
	}
	
	function tick() {
	  ReactDOM.render(
	    <Clock date={new Date()} />,
	    document.getElementById('root')
	  );
	}
	
	setInterval(tick, 1000);
	
The setInterval function will call tick every 1 second (1000 milliseconds).  The tick function will render a new clock, with a new date.  

This will run, but it would be better if tick and setInterval were defined and called inside the clock.  If someone wanted to use the clock function outside of the file it was defined in, they would have to call setInterval(tick, 1000) in their own code.  Ideally, we would want users to be able to create a new clock like this:

	ReactDOM.render(
	  <Clock />,
	  document.getElementById('root')
	);
	
This way, they can create a clock without needing to know about setInterval or tick.  To do this, we will create clock as a class component, rather than a function component.  Here is how it is done:

	class Clock extends React.Component {
	  constructor(props) {
	    super(props);
	    this.state = {date: new Date()};
	  }
	
	  componentDidMount() {
	    this.timerID = setInterval(
	      () => this.tick(),
	      1000
	    );
	  }
	
	  componentWillUnmount() {
	    clearInterval(this.timerID);
	  }
	
	  tick() {
	    this.setState({
	      date: new Date()
	    });
	  }
	
	  render() {
	    return (
	      <div>
	        <h1>Hello, world!</h1>
	        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
	      </div>
	    );
	  }
	}
	
	ReactDOM.render(
	  <Clock />,
	  document.getElementById('root')
	);
	
In the constructor of a class component, we must always call super on our props.  Also, date is no longer a prop, which means we don't need a user to pass it in as a parameter.  Finally, this.state has a special meaning.  The state of a class component is meant to hold things that change frequently, and change consistantly (in a way that is predictable, like time, or when a user clicks on something).  Not everything should go in this.state, as we'll soon see.

The componentDidMount and componentWillUnmount are called lifecycle hooks.  componentDidmount is called once at the first rendering.  It passes the tick function to setInterval, and makes it tick every second.  Notice that we put the value returned by setInterval into this.timerID, which is not a part of state.  This is because the timer object returned by setInterval will never change.  Since state is made to house attributes that change frequently, and the timers ID will never change, it should not go in state.

componentWillUnmount will be called if the clock is ever removed from the DOM (the screen).  Removing the clock from the DOM doesn't stop the every-second updates, it just stops the user from seeing the clock.  If we didn't completely destroy the clock, a user who navigates to the page with the clock multiple times will end up updating a whole bunch of clocks that they can't see, potentially slowing down their system.

tick and render are the same as before.

Finally, we have ReactDOM.render (which will call Clock's render function).  Notice that now all you have to do to set up a new clock is say `<Clock />`.  No need to explicitly call setInterval or reference tick.

<details><summary>Component State</summary>

As we saw in the clock example, only things that get modified frequently and consistently should go in the state.

Do not modify the state directly.  For example, this will not re-render a component:

	// Wrong
	this.state.comment = 'Hello';

Instead, use setState():

	// Correct
	this.setState({comment: 'Hello'});

The only place where you can assign this.state is the constructor.

`React state updates are asynchronous`  
this.props and this.state may be updated asynchronously. you should not rely on their values for calculating the next state.

For example, this code may fail to update the counter:

	// Wrong
	this.setState({
	  counter: this.state.counter + this.props.increment,
	});

Think of calling setState as enquing an update, not necessarily performing it right away.  If this.state.counter = 0, and this.props.increment = 1, you would expect that calling this.setState 10 times would result in this.state.counter = 10.  However, it is possible that the addition this.state.counter + this.props.increment may happen without reassigning to counter right away.  Then you will have enqued 10 call to make this.state.counter = 0 + 1, and the final result will be 1.

If you pass setState a function, it behaves differently.  setState will give the triplet (state, props, context) to your function.  state, etc are potentially different from this.state, etc, as they are handed to your function from the previous function in the queue.

	// Correct
	this.setState((prevState, props) => ({
	  counter: prevState.counter + props.increment
	}));
	
Only providing 2 parameters is fine, context will just be ignored.

</details>

</details>

`preventDefault`
Stops the default behavior of most things.  The default behavior of clicking a link is to redirect to that page.  You can prevent this and do something else when the link is clicked.

`Synthetic Event`
Events are different depending on what browser you're using.  Synthetic events wrap around these different events to make a uniform interface.

<details><summary>controlled components</summary>

html form elements like input and select have their own state.  We want react to control the state to make this simple.  In react, these are called controlled components.

In pure html:

	<form>
	  <label>
	    Name:
	    <input type="text" name="name" />
	  </label>
	  <input type="submit" value="Submit" />
	</form>
	
In react:

	class NameForm extends React.Component {
	  constructor(props) {
	    super(props);
	    this.state = {value: ''};
	
	    this.handleChange = this.handleChange.bind(this);
	    this.handleSubmit = this.handleSubmit.bind(this);
	  }
	
	  handleChange(event) {
	    this.setState({value: event.target.value});
	  }
	
	  handleSubmit(event) {
	    alert('A name was submitted: ' + this.state.value);
	    event.preventDefault();
	  }
	
	  render() {
	    return (
	      <form onSubmit={this.handleSubmit}>
	        <label>
	          Name:
	          <input type="text" value={this.state.value} onChange={this.handleChange} />
	        </label>
	        <input type="submit" value="Submit" />
	      </form>
	    );
	  }
	}
	
</details>

#Chrome Devtools

Opened in chrome by right clicking on a page then clicking inspect.

`element`

`console`

`sources`

`network`

#Misc
https://notes.shichao.io/

check out this weebs notes.  not bad at all.

this is a record of you trying your best to learn.  even if it doesn't work out, at least you can say, with undenyable proof, that you tried.

remember: learn top down, not bottom up.  what problem does this solve, how does it fit into the larger problem you're trying to solve, what are its keywords/components.
