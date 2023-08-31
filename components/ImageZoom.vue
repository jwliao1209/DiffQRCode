<script setup lang="ts">
// https://github.com/francoischalifour/medium-zoom/blob/master/examples/vue/src/components/ImageZoom.vue
import {  watch, type ImgHTMLAttributes, type ComponentPublicInstance } from 'vue'
import mediumZoom, { type Zoom, type ZoomOptions } from 'medium-zoom'

interface Props extends /* @vue-ignore */ ImgHTMLAttributes {
  options?: ZoomOptions
}

const props = defineProps<Props>()

let zoom: Zoom | null = null

function getZoom() {
  if (zoom === null) {
    zoom = mediumZoom(props.options)
  }

  return zoom
}

function attachZoom(ref: Element | ComponentPublicInstance | null) {
  const image = ref as HTMLImageElement | null
  const zoom = getZoom()

  if (image) {
    zoom.attach(image)
  } else {
    zoom.detach()
  }
}

watch(() => props.options, (options) => {
  const zoom = getZoom()
  zoom.update(options || {})
})
</script>

<template>
  <img :ref="attachZoom" />
</template>

<style scoped>
.medium-zoom-overlay,
.medium-zoom-image--opened {
  z-index: 999;
  border-radius: 0;
  border: initial;
}
img {
  border-radius: 5px;
  width: 100%;
}

@media (prefers-color-scheme: light) {
  img {
    border: solid 0.1rem #ddd;
  }
}
</style>

