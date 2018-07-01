<a class="prev" onclick="plusSlidesSLIDESHOWNUM(-1)">&#10094;</a>
<a class="next" onclick="plusSlidesSLIDESHOWNUM(1)">&#10095;</a>

<div class="mySlidesSLIDESHOWNUM">
  <div>1 / NUMSLIDES</div>
  SLIDE 1
</div>

<div class="mySlidesSLIDESHOWNUM">
  <div>2 / NUMSLIDES</div>
  SLIDE 2
</div>

<div class="mySlidesSLIDESHOWNUM">
  <div>3 / NUMSLIDES</div>
  SLIDE 3
</div>

<script>
var slideIndex = 1;
showSlidesSLIDESHOWNUM(slideIndex);

function plusSlidesSLIDESHOWNUM(n) {
  showSlidesSLIDESHOWNUM(slideIndex += n);
}

function showSlidesSLIDESHOWNUM(n) {
  var i;
  var slides = document.getElementsByClassName("mySlidesSLIDESHOWNUM");
  if (n > slides.length) {slideIndex = 1}    
  if (n < 1) {slideIndex = slides.length}
  for (i = 0; i < slides.length; i++) {
      slides[i].style.display = "none";  
  }
  slides[slideIndex-1].style.display = "block";
}
</script>