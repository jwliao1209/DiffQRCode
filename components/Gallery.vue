<script setup>
import { useResizeObserver } from '@vueuse/core'

const props = defineProps({
  urls: {
    tyle: Array,
    required: true,
  }
})

const videoIndex = ref(0)

function incrementIndex (val) {
  videoIndex.value += val
  if (videoIndex.value >= props.urls.length) {
    videoIndex.value -= props.urls.length
  } else if (videoIndex.value < 0) {
    videoIndex.value += props.urls.length
  }
}

// set a dynamic min-height to prevent flickering on tab switches.
const minHeight = ref('0px')
const main = ref(null)
function setHeight () {
  minHeight.value = '0px'
  nextTick(() => {
    minHeight.value = getComputedStyle(main.value).getPropertyValue('height')
  })
}
useResizeObserver(main, () => setHeight())

</script>

<template>
<div>
  <div ref="main" class="main" :style="{ minHeight: minHeight }">
    <div class="navigate-button left-button" @click="incrementIndex(-1)"></div>
    <div class="video">
      <Transition name="fade" mode="out-in">
      <VideoComparison :key="videoIndex" :url="props.urls[videoIndex]" @ready="setHeight" />
      </Transition>
    </div>
    <div class="navigate-button right-button" @click="incrementIndex(1)"></div>
  </div>
  <div class="tab-list">
    <div v-for="index in props.urls.length" @click="videoIndex = index - 1">
      <div v-if="index - 1 === videoIndex"></div>
    </div>
  </div>
</div>
</template>

<style scoped>
.tab-list {
  margin-top: 0.6rem;
  display: flex;
  justify-content: center;
}
.tab-list > div {
  width: 1.5%;
  aspect-ratio: 1/1;
  border-radius: 50%;
  background-color: rgba(193, 193, 198, 0.655);
  margin: 0 0.3%;
  cursor: pointer;
  transition: transform .1s;
  transition: background-color .5s;
  margin-bottom: 1rem;
}
.tab-list > div:active {
  transform: scale(0.93);
}
.tab-list > div > div {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, .56);
}
@media (prefers-color-scheme: dark) {
  .tab-list > div > div {
    background-color: rgba(255, 255, 255, 1);
  }
}

.main {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.video {
  width: 86%;
}
.navigate-button {
  cursor: pointer;
  width: 5%;
  aspect-ratio: 1/1;
  font-size: 2.5vw;
  border-radius: 50%;
  background-color: rgba(193, 193, 198, 0.655);
  color: rgba(0,0,0,.56);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform .1s;
}
@media (min-width: 1280px) {
  .navigate-button {
    font-size: 32px;
  }
}
.navigate-button:active {
  transform: scale(0.93);
}
.left-button:after {
  font: var(--fa-font-solid);
  content: '\f104';
}
.right-button:after {
  font: var(--fa-font-solid);
  content: '\f105';
}
.fade-enter-active,
.fade-leave-active {
  transition: all .3s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

