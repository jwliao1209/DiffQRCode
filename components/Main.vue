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
    <div class="image">
      <ImageZoom src="images/teaser.png" :options="{ background: imageOverlayColor }" />
      <div class="caption">Leveraging the preeminent capability of Latent Diffusion Model (LDM) and ControlNet as a prior knowledge of aesthetic QR code images, coupled with our proposed Scanning-Robust (Perceptual) Guidance, we can generate custom-styled QR codes conform to user prompts while assuring both scannability and aesthetics.</div>
    </div>

    <div class="section-title">Abstract</div>
    <p>
      QR codes, prevalent in daily applications, lack visual appeal due to their conventional black-and-white design. Integrating aesthetics while maintaining scannability poses a challenge. In this paper, we introduce a novel diffusion-model-based aesthetic QR code generation pipeline, utilizing pre-trained ControlNet and guided iterative refinement via a novel classifier guidance (SRG) based on the proposed Scanning-Robust Loss (SRL) tailored with QR code mechanisms, which ensures both aesthetics and scannability. To further improve the scannability while preserving aesthetics, we propose a two-stage pipeline with Scanning-Robust Perceptual Guidance (SRPG). Moreover, we can further enhance the scannability of the generated QR code by postprocessing it through the proposed Scanning-Robust Projected Gradient Descent (SRPGD) post-processing technique based on SRL with proven convergence. With extensive quantitative, qualitative, and subjective experiments, the results demonstrate that the proposed approach can generate diverse aesthetic QR codes with flexibility in detail. In addition, our pipelines outperforming existing models in terms of Scanning Success Rate (SSR) 86.67% (+40%) with comparable aesthetic scores. The pipeline combined with SRPGD further achieves 96.67% (+50%).
    </p>
    <!-- <div class="image">
      <ImageZoom src="/images/girl.jpg" :options="{ background: imageOverlayColor }" />
      <div class="caption">We extract a 20-pixel-wide vertical segment of pixels from each generated frame and stack them horizontally. MeDM produces fluent videos which reconstruct stripe-free images.
        <a @click="showGirlVideo = !showGirlVideo">
        <template v-if="!showGirlVideo">Show</template>
        <template v-else>Hide</template>
        video.</a>
      </div>
    </div> -->
    <!-- <VideoComparisonMultiple v-if="showGirlVideo"
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
    /> -->

    <div class="section-title">Methodology</div>

    <div class="image">
      <ImageZoom src="images/loss.png" :options="{ background: imageOverlayColor }" />
      <div class="caption"><span class="bold">Scanning-Robust Loss (SRL).</span> We emulate the scanning process using module pixel extraction and binarization to calculate the pixel-wise error matrix and module-wise optimization decision mask. Then we apply a Gaussian kernel to re-weight the error matrix. Finally, we mask the error matrix with the decision mask via Hadamard product, then take the average to form our SRL.</div>
    </div>

    <div class="image">
      <ImageZoom src="images/one_stage_generation_pipeline.png" :options="{ background: imageOverlayColor }" />
      <div class="caption"><span class="bold">Iterative refinement with Scanning Robustness Guidance (SGD).</span> First, we leverage pre-trained ControlNet to obtain the initial score prediction conditioned on the target QR code and user-specified prompt. During each denoising step, we approximate the original latent followed by DDIM formulation, then apply the VAE decoder to get the original image for SRL calculation. We utilize the gradient of SRL as a guidance term to update the predicted score. Repeat the above iterative refinement process until convergence.</div>
    </div>
    
    <div class="image">
      <ImageZoom src="images/two_stage_generation_pipeline.png" :options="{ background: imageOverlayColor }" />
      <div class="caption"><span class="bold">Two-stage generation pipeline with Scanning-Robust Perceptual Guidance (SRPG).</span> In Stage 1, we utilize the pre-trained plain ControlNet to generate an aesthetic yet unscannable sub-optimal QR code; In Stage 2, we first perform SDEdit to convert the sub-optimal QR code to latent space, then leverage Qart to merge with the target QR code, finally, we apply our proposed iterative refinement to produce aesthetic and scannable QR code.</div>
    </div>

    <div class="section-title">Comparison Results</div>

    <div class="image">
      <ImageZoom src="images/comparison.png" :options="{ background: imageOverlayColor }" />
      <div class="caption">Comparisons with generative-based methods. The green box represents scannable images, while the red box indicates images that cannot be scanned.</div>
    </div>

    <div class="image">
      <ImageZoom src="images/compare.png" :options="{ background: imageOverlayColor }" />
      <div class="caption">Quantitative results of generative-based methods and our proposed pipeline. Improvements marked in green are compared with QR Code Monster.</div>
    </div>

    <!-- <p>
    MeDM is capable of efficiently rendering high quality videos solely from 3D assets, including optical flows, occlusions and position information (depth, normal). We use the lineart derived from the normal maps as the input conditions to ControlNet. 3D assets from <a href="http://sintel.is.tue.mpg.de" target="_blank">MPI Sintel</a>.
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
    /> -->
  <div class="section-title">Analytics</div>
    <p>
      In Fig. (a), we compare the error rates of a sample with different SRG weights during iterative refinement steps. We observed that the error plunges within the first 5 iterations with SRG, whereas without SRG. Furthermore, we analyze the change in score magnitude of different SRG weights. We found that the score magnitude decreased over the iterations, indicating the guidance effects diminished over time. This trend is depicted in Fig. (b).
    </p>
    
    <div style="display: flex;">
      <div class="image">
        <ImageZoom src="images/error.png" :options="{ background: imageOverlayColor }" />
        <div class="caption">(a) QR code error rate.</div>
      </div>
      <div class="image">
        <ImageZoom src="images/gradient_norm.png" :options="{ background: imageOverlayColor }" />
        <div class="caption">(b) Score magnitude</div>
      </div>
    </div>
    
    <p>
      We visualize the images at different timestep and their corresponding mismatched modules. The mismatched modules are marked in red, indicating the inconsistencies between 
scanner-decoded image and the target QR code, Initially, the image contains a plethora of mismatched modules, leading to the unscannable situation. However, the number of mismatched modules significantly decreases as the sampling process proceeds. Moreover, we can observe that the amount of mismatched modules plunges after certain sampling steps. This indicates that the mismatch rate falls within the QR code error-correction capacity, allowing the control reverting to the diffusion model to generate more appealing results.
    </p>

    <div class="image">
      <ImageZoom src="images/iterative_refinement_process.png" :options="{ background: imageOverlayColor }" />
    </div>

    <p>
      We analyze the robustness of the generated results through error analysis. The scanning robustness can be maintained as long as the modules after sampling and binarization yield identical results as the target QR code regardless of pixel color changes within the modules. Our aesthetic QR codes exhibit irregular colors and shapes in their modules. Despite undergoing sampling and binarization, the module results remain consistent with the original QR code. This suggests that our aesthetic QR codes are robust and readable by a standard QR code scanner.
    </p>

    <div class="image">
      <ImageZoom src="images/error_analysis_qrcode_module_error.png" :options="{ background: imageOverlayColor }" />
    </div>
    <!-- <Gallery
      :urls="[
        '/videos/edit/bear',
        '/videos/edit/blackswan',
        '/videos/edit/boat',
        '/videos/edit/flamingo',
      ]"
      :captions="[
        'Prompt: a panda in Arctic, snow, ice, iceberg',
        'Prompt: a white swan in the ocean, snowing',
        'Prompt: a boat on fire',
        'Prompt: flamingos in outer space',
      ]"
    /> -->

    <!-- <div class="section-title">Video Anonymization</div>
    <p>
      Finally, we demonstrate the versatility of MeDM. For example, MeDM can perform video anonymization out-of-the-box. We leverage the fact that human visual perception exhibits a remarkable sensitivity to human faces while our ability to detect and recognize other objects is not as specialized. We add noise to a video with a strength of 0.5T, which is strong enough to erase the identity while preserving other objects and the background scene, and perform denoising using MeDM to obtain the anonymized video. Text conditioning can also be injected to enable a more targeted identity modification. Celebrity videos from <a href="https://celebv-hq.github.io" target="_blank">CelebV-HQ</a>.
    </p>
    <Gallery
      :urls="shuffle([
        '/videos/anonymization/anna-kendrick',
        '/videos/anonymization/chris-hemsworth',
        '/videos/anonymization/mark-zuckerberg',
        '/videos/anonymization/bill-gates',
        '/videos/anonymization/dicaprio-n-obama',
      ])"
    /> -->

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
  /* line-height: 0; */
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

.bold{
  font-weight: bold;
}
</style>

