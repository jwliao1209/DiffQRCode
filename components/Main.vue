<template>
  <div class="main">
    <div class="video">
      <VideoComparison url="/videos/teaser" />
      <div class="caption">MeDM enables temporally consistent video rendering and translation using image Diffusion Models. Slide for interactive comparison. Inputs are on the left.</div>
    </div>

    <!--
    Abstract
    <div>
      This study introduces an efficient and effective method, MeDM, that utilizes pre-trained image Diffusion Models for video-to-video translation with consistent temporal flow. The proposed framework can render videos from scene position information, such as a normal G-buffer, or perform text-guided editing on videos captured in real-world scenarios. We employ explicit optical flows to construct a practical coding that enforces physical constraints on generated frames and mediates independent frame-wise scores. By leveraging this coding, maintaining temporal consistency in the generated videos can be framed as an optimization problem with a closed-form solution. To ensure compatibility with Stable Diffusion, we also suggest a workaround for modifying observed-space scores in latent-space Diffusion Models. Notably, MeDM does not require fine-tuning or test-time optimization of the Diffusion Models. Through extensive qualitative, quantitative, and subjective experiments on various benchmarks, the study demonstrates the effectiveness and superiority of the proposed approach.
    </div>
    <ImageZoom src="/images/girl.jpg" />
    -->
  </div>
</template>

<style scoped>
.main {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem
}
.video .caption {
  display: flex;
  justify-content: center;
  margin-top: .2rem;
  padding: 0 2.5rem;
  font-size: 1.2rem;
  text-align: center;
}
@media (max-width: 650px) {
  .video .caption {
    font-size: 1rem;
    padding: 0;
  }
}
</style>

