<script setup>
useHead({
  title: 'MeDM: Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance',
  meta: [
    {
      name: 'description',
      content: 'MeDM utilizes pre-trained image Diffusion Models for video-to-video translation with consistent temporal flow. The proposed framework can render videos from scene position information, such as a normal G-buffer, or perform text-guided editing on videos captured in real-world scenarios. We employ explicit optical flows to construct a practical coding that enforces physical constraints on generated frames and mediates independent frame-wise scores. By leveraging this coding, maintaining temporal consistency in the generated videos can be framed as an optimization problem with a closed-form solution. To ensure compatibility with Stable Diffusion, we also suggest a workaround for modifying observed-space scores in latent-space Diffusion Models. Notably, MeDM does not require fine-tuning or test-time optimization of the Diffusion Models.',
    },
    {
      name: "google-site-verification",
      content: "I9VHWgYS9g3o8RUJ7ryB1hPoIcN9YK1bX1Ds7_uDIYE",
    },
  ],
})

useSeoMeta({
  title: 'MeDM: Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance',
  ogTitle: 'MeDM: Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance',
  description: 'MeDM utilizes pre-trained image Diffusion Models for video-to-video translation with consistent temporal flow. The proposed framework can render videos from scene position information, such as a normal G-buffer, or perform text-guided editing on videos captured in real-world scenarios. We employ explicit optical flows to construct a practical coding that enforces physical constraints on generated frames and mediates independent frame-wise scores. By leveraging this coding, maintaining temporal consistency in the generated videos can be framed as an optimization problem with a closed-form solution. To ensure compatibility with Stable Diffusion, we also suggest a workaround for modifying observed-space scores in latent-space Diffusion Models. Notably, MeDM does not require fine-tuning or test-time optimization of the Diffusion Models.',
  ogDescription: 'MeDM utilizes pre-trained image Diffusion Models for video-to-video translation with consistent temporal flow. The proposed framework can render videos from scene position information, such as a normal G-buffer, or perform text-guided editing on videos captured in real-world scenarios. We employ explicit optical flows to construct a practical coding that enforces physical constraints on generated frames and mediates independent frame-wise scores. By leveraging this coding, maintaining temporal consistency in the generated videos can be framed as an optimization problem with a closed-form solution. To ensure compatibility with Stable Diffusion, we also suggest a workaround for modifying observed-space scores in latent-space Diffusion Models. Notably, MeDM does not require fine-tuning or test-time optimization of the Diffusion Models.',
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

