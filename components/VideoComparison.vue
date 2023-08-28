<script setup>
// Partly ported from http://thenewcode.com/364/Interactive-Before-and-After-Video-Comparison-in-HTML5-Canvas

import { ref } from 'vue'
const props = defineProps(['url'])

Number.prototype.clamp = function (min, max) {
  return Math.min(Math.max(this, min), max)
}

const video = ref(null)
const canvas = ref(null)
let rendered = false
function setupSlider () {
  // we have to make this function triggered on `timeupdate`
  // if we use `play` event, vue sometimes fails to capture the event during
  //   local development (probably because the video plays before vue is instantiated)
  if (rendered) {
    return
  }
  rendered = true

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
<div class="video-and-canvas">
  <video ref="video" poster="" loop muted autoplay playsinline @timeupdate="setupSlider">
    <source :src="props.url" />
  </video>
  <canvas ref="canvas"></canvas>
</div>

</template>

<style scoped>
canvas {
  width: 100%;
  cursor: grabbing;
}
video {
  /*
    Hide video without stopping it.
    Most of the browsers pause the video when it goes out of view.
    So we make it positioned at the middle of the canvas, vertically,
    and use negative z-index to hide it.
    Additionally, the video has to take up some pixels to be played by the browsers,
    so we make its height 1px.
  */
  position: absolute;
  top: 50%;
  z-index: -1;
  height: 1px;
}
.video-and-canvas {
  position: relative;
}
/*
canvas {
  border: solid;
}
*/
</style>
