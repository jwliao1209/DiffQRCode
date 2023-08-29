<script setup>
import { computed } from 'vue'

const isDark = usePreferredDark()
const imageOverlayColor = computed(() => {
  const foo = isDark.value // make imageOverlayColor reactive
  const style = getComputedStyle(document.body)
  const color = style.getPropertyValue('--color-background')
  return color
})

</script>

<template>
  <div class="main">
    <div class="video">
      <VideoComparison url="/videos/teaser" />
      <div class="caption">MeDM enables temporally consistent video rendering and translation using image Diffusion Models. Slide for interactive comparison. Inputs are on the left.</div>
    </div>

    <div class="section-title">Abstract</div>
    <p>
      MeDM utilizes pre-trained image Diffusion Models for video-to-video translation with consistent temporal flow. The proposed framework can render videos from scene position information, such as a normal G-buffer, or perform text-guided editing on videos captured in real-world scenarios. We employ explicit optical flows to construct a practical coding that enforces physical constraints on generated frames and mediates independent frame-wise scores. By leveraging this coding, maintaining temporal consistency in the generated videos can be framed as an optimization problem with a closed-form solution. To ensure compatibility with Stable Diffusion, we also suggest a workaround for modifying observed-space scores in latent-space Diffusion Models. Notably, MeDM does not require fine-tuning or test-time optimization of the Diffusion Models.
    </p>
    <ImageZoom class="image" src="/images/girl.jpg" :options="{ background: imageOverlayColor }" />
    <div class="caption">We extract a 20-pixel-wide vertical segment of pixels from each generated frame and stack them horizontally. MeDM produces fluent videos which reconstruct stripe-free images.</div>
  </div>
</template>

<style scoped>
.main {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem
}
.section-title {
  display: flex;
  justify-content: center;
  font-size: 2rem;
  font-weight: 500;
  margin: 2rem 0 0.5rem 0;
}
.caption {
  display: flex;
  justify-content: center;
  margin-top: .2rem;
  padding: 0 2.5rem;
  font-size: 1.2rem;
  text-align: center;
}
.image {
  width: 100%;
}
@media (max-width: 650px) {
  .caption {
    font-size: 1rem;
    padding: 0;
  }
}
p {
  margin: .5rem 0 1rem 0;
}
</style>

