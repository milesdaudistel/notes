Always keep this in mind:  Writing notes is a physical record of your attempt to understand something.  Even if all your studies end up fruitless, you can at least prove to yourself that you tried your best.

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
		
		public int Get():
			return bork
			
Now everyone can use Get to see a copy of your bork.  They will be able to change the copy of their bork, but they won't be able to change yours.  So that's getters.

What about setters?  Lets say now that someone can ask doggo to quiet down by setting his bork to a lower volume.

	class Doggo:
		public int bork = 4
		
But what if someone sets doggo's bork to 0?  Then doggo would be sad because it couldn't bork at all.  And what about a negative volume?  That doesn't make any sense.  Let's introduce a setter that will stop people from doing dumb stuff like that.

	class Doggo:
		private int bork = 4
		
		public int Get():
			return bork
		public void Set(int volume):
			if volume < 1:
				print("BOOOOORK")
			else:
				bork = volume
		
		
Ok, but what is the point of setters that are just:

	class X:
		public int y = 4
		
		public void Set(int z):
			y = z
		
Why not let users access those directly?  I'll answer that question with a question: what if we later decide that we wanted to change the functionality of how a user is able to set a variable?  If we make x private, that would break everyone's code that is currently calling X.y.  So from the start, we should use a setter function so that people who want to use this class call X.Set(3).  This way, when we want to make a change, we don't break a bunch of other stuff.
 

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
The function keyword can be used to define a function inside an expression

	var getRectArea = function(width, height) {
	    return width * height;
	}
	
  componentDidMount() {
    this.timerID = setInterval(
      this.tick,
      1000
    );
  }
  
  this.tick doesn't get called.  why?	


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

Here's an example with components in components:

	function Welcome(props) {
	  return <h1>Hello, {props.name}</h1>;
	}
	
	function App() {
	  return (
	    <div>
	      <Welcome name="Sara" />
	      <Welcome name="Cahal" />
	      <Welcome name="Edite" />
	    </div>
	  );
	}
	
	ReactDOM.render(
	  <App />,
	  document.getElementById('root')
	);

Here, App outputs a welcome for each person.


clock example
clock is a function
	we want it to be self contained
	the date shouldn't be a prop, clock should be able to modify it
	shouldn't have to call interval, should also be internal to clock
turn it into a class
	but now that it's an object of class Clock, we need special methods for first time rendering.  use componentDidMount
	need to clear the timer too.  componentWillUnmount.  wait, what's the point of this?  if we leave the page, won't the clock componenet just die anyway?
	difference between state and just local variable?  Ah, state is for things that change frequently, local variables are for things that don't change frequently.  More efficient this way.  Since time updates every second, we want to put it in state.  State is for frequent, predictable changes.
	
() => function()

is this a function call or a function definition?

</details>

TODO add stuff about american governemnt
