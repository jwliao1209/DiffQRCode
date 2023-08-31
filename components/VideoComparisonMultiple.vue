<script setup>
const props = defineProps({
  urls: {
    tyle: Array,
    required: true,
  },
  captions: Array,
})

console.assert(
  !(props.urls && props.captions && props.urls.length !== props.captions.length),
  'passed urls and captions should have the same length'
)

const allReady = ref(false)
const readyStates = {}
props.urls.forEach(url => { readyStates[url] = false })
function changeReadyStates (url) {
  readyStates[url] = true
  if (Object.values(readyStates).every(value => value)) {
    allReady.value = true
  }
}

const syncSlider = ref(true)
const commonSliderPosition = ref(0.5)
function updateCommonSliderPosition (position) {
  commonSliderPosition.value = position
}
</script>

<template>
  <div>
    <div class="captions">
      <div v-for="caption in props.captions">
        {{ caption }}
        <canvas width="10000" height="1"></canvas>
      </div>
    </div>
    <div class="main">
      <div class="video" v-for="(url, index) in props.urls">
        <VideoComparison
          :url="url" noautoplay
          :playOnChange="allReady" @ready="changeReadyStates"
          :syncSlider="syncSlider" :sliderPosition="commonSliderPosition"
          @sliderPositionChange="updateCommonSliderPosition"
        />
      </div>
    </div>
    <div class="options">
      <input type="checkbox" id="sync" v-model="syncSlider">
      <label for="sync">Sync slider</label>
    </div>
  </div>
</template>

<style scoped>
.captions {
  display: flex;
  align-items: flex-end;
  gap: 0.5rem;
}
.captions > div {
  font-weight: 500;
  text-align: center;
}
.captions > div > canvas {
  display: block;
  width: 100%;
}
.main {
  display: flex;
  gap: 0.5rem;
}
.options {
  display: flex;
  gap: 0.3rem;
  margin-top: 0.3rem;
  align-items: center;
  justify-content: center;
}
@media (min-width: 850px) {
  input[type="checkbox"] {
    zoom: 1.5;
  }
}
@media (max-width: 450px) {
  input[type="checkbox"] {
    zoom: 0.8;
  }
}
</style>
