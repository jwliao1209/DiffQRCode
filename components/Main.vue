<script setup>
const mounted = ref(false)
onMounted(() => {
  mounted.value = true
})

const isDark = usePreferredDark()
const imageOverlayColor = computed(() => {
  const foo = isDark.value // make imageOverlayColor reactive to dark mode
  if (!mounted.value) {
    return '' 
  }
  const style = window.getComputedStyle(document.body)
  const color = style.getPropertyValue('--color-background')
  return color
})

const showGirlVideo = ref(false)

function shuffle(array) {
  let currentIndex = array.length,  randomIndex;
  // While there remain elements to shuffle.
  while (currentIndex > 0) {
    // Pick a remaining element.
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex--;
    // And swap it with the current element.
    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex], array[currentIndex]];
  }
  return array;
}
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
    <div class="image">
      <ImageZoom src="/images/girl.jpg" :options="{ background: imageOverlayColor }" />
      <div class="caption">We extract a 20-pixel-wide vertical segment of pixels from each generated frame and stack them horizontally. MeDM produces fluent videos which reconstruct stripe-free images.
        <a @click="showGirlVideo = !showGirlVideo">
        <template v-if="!showGirlVideo">Show</template>
        <template v-else>Hide</template>
        video.</a>
      </div>
    </div>
    <VideoComparisonMultiple v-if="showGirlVideo"
      :urls="[
        '/videos/girl/sde',
        '/videos/girl/cn',
        '/videos/girl/sde-medm',
        '/videos/girl/cn-medm'
      ]"
      :captions="[
        'SDEdit',
        'ControlNet',
        'SDEdit + MeDM',
        'ControlNet + MeDM',
      ]"
    />

    <div class="section-title">Architecture</div>
    <div class="image">
      <ImageZoom src="/images/system-diagram.jpg" :options="{ background: imageOverlayColor }" />
      <div class="caption">MeDM mediates independent image score estimations after every denoising step. Inspired by the fact that video pixels are essentially views to the underlying objects, we construct an explicit pixel repository <LatexR /> to represent the underlying world. For more details, please refer to our <a href="/medm.pdf" target="_blank">paper</a>.</div>
    </div>

    <div class="section-title">Video Rendering</div>
    <p>
      MeDM is capable of efficiently rendering high quality videos solely from 3D assets, including optical flows, occlusions and position information (depth, normal). We use the lineart derived from the normal maps as the input conditions to ControlNet.
    </p>
    <Gallery
      :urls="shuffle([
        '/videos/rendering/ambush_7',
        '/videos/rendering/bandage_2',
        '/videos/rendering/market_5',
        '/videos/rendering/shaman_3',
        '/videos/rendering/cave_2',
        '/videos/rendering/market_6',
        '/videos/rendering/temple_2',
      ])"
    />

    <p>
      In addition, MeDM can also begin with adding noise to the pre-rendered videos and perform the denoising process from 0.5T step following SDEdit. The generated video should be similar to the original animation and incorporating the realistic prior from the pre-trained DM (The difference is especially significant in complex texture, such as hair). More quantitative results, including comparison with prior works, can be found in our <a href="/medm.pdf" target="_blank">paper</a>. 
    </p>
    <Gallery
      :urls="shuffle([
        '/videos/assistive-rendering/ambush_7',
        '/videos/assistive-rendering/bandage_2',
        '/videos/assistive-rendering/market_5',
        '/videos/assistive-rendering/ambush_4',
        '/videos/assistive-rendering/ambush_6',
        '/videos/assistive-rendering/shaman_2',
        '/videos/assistive-rendering/temple_3',
      ])"
    />

    <div class="section-title">Text-Guided Video Edit</div>
    <p>
      MeDM also performs well without high precision optical flows. We demonstrate this by applying text-guided video editing on real-world videos in DAVIS 2016.
    </p>

    <Gallery
      :urls="shuffle([
        '/videos/edit/bear',
        '/videos/edit/blackswan',
        '/videos/edit/boat',
        '/videos/edit/flamingo',
      ])"
    />

    <div class="section-title">Video Anonymization</div>
    <p>
      Finally, we demonstrate the versatility of MeDM. For example, MeDM can perform video anonymization out-of-the-box. We leverage the fact that human visual perception exhibits a remarkable sensitivity to human faces while our ability to detect and recognize other objects is not as specialized. We add noise to a video with a strength of 0.5T, which is strong enough to erase the identity while preserving other objects and the background scene, and perform denoising using MeDM to obtain the anonymized video. Text conditioning can also be injected to enable a more targeted identity modification.
    </p>
    <Gallery
      :urls="shuffle([
        '/videos/anonymization/anna-kendrick',
        '/videos/anonymization/chris-hemsworth',
        '/videos/anonymization/mark-zuckerberg',
        '/videos/anonymization/bill-gates',
        '/videos/anonymization/dicaprio-n-obama',
      ])"
    />

  </div>
</template>

<style scoped>
.main {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem
}
.section-title {
  text-align: center;
  font-size: 2rem;
  font-weight: 500;
  margin: 2rem 0 0.5rem 0;
}
.caption {
  margin-top: .2rem;
  padding: 0 2.5rem;
  font-size: 1rem;
  text-align: center;
  line-height: 1.6;
}
.image {
  margin: 1.5rem 0 1rem 0;
  line-height: 0;
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

