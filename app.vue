<script setup>
useHead({
  title: 'MeDM',
  meta: [
    {
      name: 'description',
      content: 'MeDM: Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance',
    },
    {
      name: "google-site-verification",
      content: "I9VHWgYS9g3o8RUJ7ryB1hPoIcN9YK1bX1Ds7_uDIYE",
    },
  ],
})

useSeoMeta({
  title: 'MeDM',
  ogTitle: 'MeDM',
  description: 'MeDM: Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance',
  ogDescription: 'MeDM: Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance',
  ogImage: 'https://medm2023.github.io/images/system-diagram.jpg',
  twitterCard: 'summary_large_image',
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

