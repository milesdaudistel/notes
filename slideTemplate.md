Use pre to make it preformatted, which will make it inside of a code block

<a class="prev" onclick="plusSlidesSLIDESHOWNAME(-1)">&#10094;</a>
<a class="next" onclick="plusSlidesSLIDESHOWNAME(1)">&#10095;</a>

<div class="mySlidesSLIDESHOWNAME">
  <div>1 / NUMSLIDES</div>
  <pre>
  SLIDE 1
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>2 / NUMSLIDES</div>
  <pre>
  SLIDE 2
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>3 / NUMSLIDES</div>
  <pre>
  SLIDE 3
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>4 / NUMSLIDES</div>
  <pre>
  SLIDE 4
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>5 / NUMSLIDES</div>
  <pre>
  SLIDE 5
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>6 / NUMSLIDES</div>
  <pre>
  SLIDE 6
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>7 / NUMSLIDES</div>
  <pre>
  SLIDE 7
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>8 / NUMSLIDES</div>
  <pre>
  SLIDE 8
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>9 / NUMSLIDES</div>
  <pre>
  SLIDE 9
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>10 / NUMSLIDES</div>
  <pre>
  SLIDE 10
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>11 / NUMSLIDES</div>
  <pre>
  SLIDE 11
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>12 / NUMSLIDES</div>
  <pre>
  SLIDE 12
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>13 / NUMSLIDES</div>
  <pre>
  SLIDE 13
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>14 / NUMSLIDES</div>
  <pre>
  SLIDE 14
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>15 / NUMSLIDES</div>
  <pre>
  SLIDE 15
  </pre>
</div>

<div class="mySlidesSLIDESHOWNAME">
  <div>16 / NUMSLIDES</div>
  <pre>
  SLIDE 16
  </pre>
</div>

<script>
var slideIndex = 1;
showSlidesSLIDESHOWNAME(slideIndex);

function plusSlidesSLIDESHOWNAME(n) {
  showSlidesSLIDESHOWNAME(slideIndex += n);
}

function showSlidesSLIDESHOWNAME(n) {
  var i;
  var slides = document.getElementsByClassName("mySlidesSLIDESHOWNAME");
  if (n > slides.length) {slideIndex = 1}    
  if (n < 1) {slideIndex = slides.length}
  for (i = 0; i < slides.length; i++) {
      slides[i].style.display = "none";  
  }
  slides[slideIndex-1].style.display = "block";
}
</script>