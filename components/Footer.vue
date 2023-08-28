<template>
<div class="footer">
  <div class="content">
    <div class="logo">
      <img src="/images/logos/as-white.png" alt="">
    </div>
    <div>
      This website is licensed under a Creative Commons Attribution-ShareAlike 4.0 International <a href="https://creativecommons.org/licenses/by-sa/4.0/" target="_blank">License</a>.
      <br />
      <br />
      This means you are free to borrow the <a href="https://github.com/medm2023/medm2023.github.io" target="_blank">source code</a> of this website, we just ask that you link back to this page in the footer. However, the copyright to the logo of Academia Sinica and the footer theme is reserved. Please remove them if your are not affiliated with Academia Sinica.
    </div>
  </div>
</div>
</template>

<style scoped>
.footer:before {
  background-image: url(/images/backgrounds/wave.png);
  content: " ";
  position: absolute;
  top: -187px;
  background-repeat: no-repeat;
  background-position: center bottom;
  background-size: cover;
  width: 100%;
  height: 188px;
  pointer-events: none;
}
.footer {
  position: relative;
  color: var(--color-text-accent);
  background-image: url(/images/backgrounds/footer.jpg);
  background-size: cover;
  background-position: top;
  background-repeat: no-repeat;
  background-color: var(--color-background-accent);
  margin-top: 100px;
  min-height: 300px;
  width: 100vw;
}
.footer a {
  color: var(--color-text-accent);
}
.content {
  position: relative;
  z-index: 1;
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
}
.logo {
  display: flex;
  place-items: flex-start;
  padding-bottom: calc(var(--section-gap) / 4);
}
img {
  width: 200px;
}

@media (min-width: 900px) {
  .footer {
    display: flex;
    place-items: center;
  }

  .content {
    display: grid;
    grid-template-columns: 1fr 3fr;
    padding: 0 2rem;
  }

  .logo {
    padding-bottom: 0;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
