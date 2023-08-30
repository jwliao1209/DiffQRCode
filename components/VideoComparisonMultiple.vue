<script setup>
// Partly ported from http://thenewcode.com/364/Interactive-Before-and-After-Video-Comparison-in-HTML5-Canvas
const props = defineProps({
  urls: Array,
  captions: Array,
})

console.assert(
  !(props.urls && props.captions && props.urls.length !== props.captions.length),
  'passed urls and captions should have the same length'
)
</script>

<template>
  <div>
    <div class="main">
      <div class="video" v-for="(url, index) in props.urls">
        {{ props.captions[index] }}
        <VideoComparison :url="url" :autoplay="false" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.main {
  display: flex;
  gap: 0.5rem;
}
.video {
  text-align: center;
}
</style>
