<a class="prev" onclick="plusSlidesINSERTNUM(-1)">&#10094;</a>
<a class="next" onclick="plusSlidesINSERTNUM(1)">&#10095;</a>

<div class="mySlidesINSERTNUM">
  <div>1 / 3</div>
  SLIDE 1
</div>

<div class="mySlidesINSERTNUM">
  <div>2 / 3</div>
  SLIDE 2
</div>

<div class="mySlidesINSERTNUM">
  <div>3 / 3</div>
  SLIDE 3
</div>

<script>
var slideIndex = 1;
showSlidesINSERTNUM(slideIndex);

function plusSlidesINSERTNUM(n) {
  showSlidesINSERTNUM(slideIndex += n);
}

function showSlidesINSERTNUM(n) {
  var i;
  var slides = document.getElementsByClassName("mySlidesINSERTNUM");
  if (n > slides.length) {slideIndex = 1}    
  if (n < 1) {slideIndex = slides.length}
  for (i = 0; i < slides.length; i++) {
      slides[i].style.display = "none";  
  }
  slides[slideIndex-1].style.display = "block";
}
</script>