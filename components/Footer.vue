<template>
<div class="footer">
  <div class="content">
    <img src="/images/logos/as-white.png" alt="">
    <div>
      This website is licensed under a <a href="https://creativecommons.org/licenses/by-sa/4.0/" target="_blank">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
      <br />
      <br />
      This means you are free to borrow the <a href="https://github.com/medm2023/medm2023.github.io" target="_blank">source code</a> of this website, we just ask that you link back to this page in the footer. However, the copyright to the logo of Academia Sinica and the footer theme is reserved. Please remove them if your are not affiliated with Academia Sinica.
    </div>
  </div>
</div>
</template>

<style scoped>
.footer::before {
  background-image: url(/images/backgrounds/wave.png);
  content: " ";
  position: absolute;
  top: -187px;
  background-repeat: no-repeat;
  background-position: center bottom;
  background-size: cover;
  width: 100%;
  height: 188px;
  z-index: 1;
  pointer-events: none;
}
.footer {
  position: relative;
  font-size: 20px;
  color: white;
  background-image: url(/images/backgrounds/footer.jpg);
  background-position: top;
  background-repeat: no-repeat;
  background-color: #153146;
}
.footer a {
  color: white;
}
.content {
  padding: 100px 100px;
}
</style>
