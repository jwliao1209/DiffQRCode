<script setup>
import { onMounted } from 'vue'
useHead({
  title: 'MeDM',
})

onMounted(() => {
  // prevent overflow-x when using 100vw with a persistant scrollbar
  const scrollbarWidth = window.innerWidth - document.body.clientWidth
  document.body.style.setProperty("--scrollbarWidth", `${scrollbarWidth}px`)
})
</script>

<template>
  <Header />
  <Main />
  <Footer />
</template>

<style>
@import url("~/assets/css/main.css");
@import url("https://cdn.jsdelivr.net/gh/jpswalsh/academicons@1/css/academicons.min.css");
@import url("https://site-assets.fontawesome.com/releases/v6.4.2/css/all.css");
</style>

