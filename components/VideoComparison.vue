<script setup>
// Partly ported from http://thenewcode.com/364/Interactive-Before-and-After-Video-Comparison-in-HTML5-Canvas
const props = defineProps({
  url: {
    type: String,
    required: true,
  },
  noautoplay: Boolean,
  playOnChange: Boolean,
  syncSlider: Boolean,
  sliderPosition: Number,
})
const emit = defineEmits(['ready', 'sliderPositionChange' ])

Number.prototype.clamp = function (min, max) {
  return Math.min(Math.max(this, min), max)
}

const video = ref(null)
const canvas = ref(null)
const showLoadingMask = ref(false)
let rendered = false

watch(() => props.playOnChange, (newVal, oldVal) => {
  if (newVal) {
    video.value.play()
  } 
})

onMounted(() => {
  canvas.value.height = 0

  // %%%%%%% Draw video poster onto canvas before video loaded %%%%%%%
  const poster = new Image()
  poster.onload = () => {
    if (canvas.value === null) {
      // the component is being unmounted
      return
    }
    const position = 0.5
    const width = poster.width / 2
    const height = poster.height
    canvas.value.width  = width
    canvas.value.height = height
    const context = canvas.value.getContext("2d")
    context.drawImage(poster, 0, 0, width, height, 0, 0, width, height)
    const compX = (width * position).clamp(0.0, width)
    const compW = (width - (width * position)).clamp(0.0, width)
    context.drawImage(poster, compX + width, 0, compW, height, compX, 0, compW, height)

    showLoadingMask.value = true
  }
  poster.src = video.value.getAttribute('poster')
})

let position = 0.5
function emitPosition () {
  if (props.syncSlider) { emit('sliderPositionChange', position) }
}
function trackLocation (e) {
  // Normalize to [0, 1]
  const bcr = canvas.value.getBoundingClientRect()
  position = ((e.pageX - bcr.x) / bcr.width)
  emitPosition()
}
function trackLocationTouch (e) {
  // Normalize to [0, 1]
  const bcr = canvas.value.getBoundingClientRect()
  position = ((e.touches[0].pageX - bcr.x) / bcr.width)
  emitPosition()
}
function setupSlider () {
  // we have to make this function triggered on `timeupdate`
  // if we use `play` event, vue sometimes fails to capture the event during
  //   local development (probably because the video plays before vue is instantiated)

  if (video.value === null || canvas.value === null) {
    // the component is being unmounted
    return
  }

  if (rendered) {
    return
  }
  rendered = true

  if (props.noautoplay) {
    video.value.pause()
  }

  emit('ready', props.url)

  const width = video.value.videoWidth / 2
  const height = video.value.videoHeight
  canvas.value.width  = width
  canvas.value.height = height
  const context = canvas.value.getContext("2d")
  if (video.value.readyState > 3) {
    // %%%%%%% Interactive %%%%%%%
    canvas.value.addEventListener("mousemove",  trackLocation, false) 
    canvas.value.addEventListener("touchstart", trackLocationTouch, false)
    canvas.value.addEventListener("touchmove",  trackLocationTouch, false)

    showLoadingMask.value = false
    // %%%%%%% Draw video onto canvas %%%%%%%
    function drawLoop () {
      if (video.value === null || canvas.value === null) {
        // the component is being unmounted
        return
      }
      if (props.syncSlider) { position = props.sliderPosition } 
      context.drawImage(video.value, 0, 0, width, height, 0, 0, width, height)
      const compX = (width * position).clamp(0.0, width)
      const compW = (width - (width * position)).clamp(0.0, width)
      context.drawImage(video.value, compX + width, 0, compW, height, compX, 0, compW, height)

      // %%%%%%% Draw split line %%%%%%%
      context.beginPath()
      context.moveTo(width * position, 0)
      context.lineTo(width * position, height)
      context.closePath()
      context.strokeStyle = "#B3993B"
      context.lineWidth = Math.round(width / 200)            
      context.stroke()

      requestAnimationFrame(drawLoop)
    }
    requestAnimationFrame(drawLoop)
  }
}

onBeforeUnmount(() => {
  canvas.value.removeEventListener("mousemove",  trackLocation, false) 
  canvas.value.removeEventListener("touchstart", trackLocationTouch, false)
  canvas.value.removeEventListener("touchmove",  trackLocationTouch, false)
})

</script>
<template>
<div class="video-and-canvas">
  <Transition name="zoom-fade">
  <div v-if="showLoadingMask" class="loading">
    <i class="fa-solid fa-spinner fa-spin"></i>
  </div>
  </Transition>
  <video ref="video" :poster="`${props.url}.jpg`" loop muted autoplay playsinline @timeupdate="setupSlider">
    <source :src="`${props.url}.mp4`" />
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
  overflow: hidden;
  line-height: 0;
  border-radius: 5px;
}
/*
canvas {
  border: solid;
}
*/
.loading {
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: color-mix(in srgb, var(--color-background) 65%, transparent);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}
.loading > i {
  font-size: 3rem;
}

.zoom-fade-leave-active {
  transition: all .5s ease-in;
}
.zoom-fade-leave-to {
  transform: scale(2);
  opacity: 0;
}
</style>
