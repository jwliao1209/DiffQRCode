<script setup>
// Partly ported from http://thenewcode.com/364/Interactive-Before-and-After-Video-Comparison-in-HTML5-Canvas

import { ref, defineProps } from 'vue'
const props = defineProps(['url'])

Number.prototype.clamp = function (min, max) {
  return Math.min(Math.max(this, min), max)
}

const video = ref(null)
const canvas = ref(null)
function setupSlider () {
  video.value.style.height = "0px"  // Hide video without stopping it

  let position = 0.5
  const width = video.value.videoWidth / 2
  const height = video.value.videoHeight
  canvas.value.width  = width
  canvas.value.height = height
  const context = canvas.value.getContext("2d")
  if (video.value.readyState > 3) {
      // %%%%%%% Interactive %%%%%%%
      function trackLocation (e) {
        // Normalize to [0, 1]
        const bcr = canvas.value.getBoundingClientRect()
        position = ((e.pageX - bcr.x) / bcr.width)
      }
      function trackLocationTouch (e) {
        // Normalize to [0, 1]
        const bcr = canvas.value.getBoundingClientRect()
        position = ((e.touches[0].pageX - bcr.x) / bcr.width)
      }
      canvas.value.addEventListener("mousemove",  trackLocation, false) 
      canvas.value.addEventListener("touchstart", trackLocationTouch, false)
      canvas.value.addEventListener("touchmove",  trackLocationTouch, false)

    // %%%%%%% Draw video onto canvas %%%%%%%
    function drawLoop () {
      context.drawImage(video.value, 0, 0, width, height, 0, 0, width, height)
      const compX = (width * position).clamp(0.0, width)
      const compW = (width - (width * position)).clamp(0.0, width)
      context.drawImage(video.value, compX + width, 0, compW, height, compX, 0, compW, height)
      requestAnimationFrame(drawLoop)

      // %%%%%%% Draw split line %%%%%%%
      context.beginPath()
      context.moveTo(width * position, 0)
      context.lineTo(width * position, height)
      context.closePath()
      context.strokeStyle = "#444444"
      context.lineWidth = 3            
      context.stroke()
    }
    requestAnimationFrame(drawLoop)

  }
}

</script>
<template>
<video ref="video" poster="" loop muted @play="setupSlider">
  <source :src="props.url" />
</video>
<canvas ref="canvas"></canvas>

</template>

<style scoped>
canvas {
  cursor: grabbing;
}
</style>
