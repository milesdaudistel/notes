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

#Databases
<details><summary>Database</summary>
A database is a way to store data in tables.  Each column is a type of data, and each row is a collection of that data.  Columns might be things like names, ages, jobs, etc.  Put these columns together and you have a table, where each row will give you the name, age, and job of a single person.
</details>

<details><summary>SQL</summary>
SQL stands for Structured Query Language.  It's a language purely for doing things with data in a database.
</details>

<details><summary>PostgreSQL</summary>
PostgreSQL is an extension of SQL.  There are many extensions for SQL like MySQL and SQLite, but PostgreSQL is considered the most modular and advanced one.  For instance, it has support for nesting, which no other SQL implementation has.  What is nesting?  Couldn't tell you.
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
