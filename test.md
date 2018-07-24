take in the name of the slideshow/pictures, and the number of slides.
produce a bunch of html slides.
produce a function that will allow you to scroll through the slides for each slideshow.

as a first step, produce a function that, when called, will produce html that will show a picture.

<script>
function my_func() {
	document.getElementById("demo").innerHTML = "<img src=\"pics/shiftreduce1.png\">";
}

my_func();

function my_func2() {
	
	var my_string = "hello" + "friend";

   var i;
   for (i = 0; i < 10; i++) {
		my_string = my_string + "hello";
   }
	document.getElementById("demo").innerHTML = my_string;
}

</script>

***

<p id="demo">
this text should not appear
</p>

***

<p id="demo2" onclick="my_func()">
clicking me will hopefully produce a lot of hellos
</p>

<script type="template" id="first_template">

</script>








