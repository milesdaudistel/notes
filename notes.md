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
	go.
	
//TODO provide an example that shows that it's more mutable.  People calling the code won't have to go back and change it.
 

</details>

#Databases

TODO change this to be a quiz type thing.  You should be able to look at the summary and think about the contents without every seeing the contents.

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

#Financial Wizardry
You need money, right? Quick advice: invest in ETFs, put money into your 401k, then your roth ira. Below is a chart from reddit. It's a little small, maybe open it separately.

<details><summary>Personal Finances Chart</summary>
![financechart](pics/financechart.jpg)
</details>


