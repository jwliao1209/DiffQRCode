<script setup>
import { reactive } from 'vue'

const config = reactive({
  title: 'MeDM: Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance',
  authors: [
    { text: 'Ernie Chu', homepage: 'https://ernestchu.github.io', mark: '' },
    { text: 'Tzuhsuan Huang', homepage: '', mark: '' },
    { text: 'Shuo-Yen Lin', homepage: '', mark: '' },
    { text: 'Jun-Cheng Chen', homepage: 'https://www.citi.sinica.edu.tw/pages/pullpull', mark: '' },
  ],
  affiliations: [
    { text: 'Research Center for Information Technology Innovation, Academia Sinica', mark: '' }
  ],
  links: [
    { text: 'Paper', url: '/medm.pdf', icon: '' },
    { text: 'arXiv', url: 'https://arxiv.org/abs/2308.10079', icon: '' },
    { text: 'Code', url: '', icon: '' },
  ],
})

</script>

<template>
  <div>
    <div class="title">{{ config.title }}</div>

    <div class="author-list">
      <div class="author" v-for="author in config.authors">
        <a :href="author.homepage" v-if="author.homepage" target="_blank">{{ author.text }}</a>
        <template v-else>{{ author.text }}</template>
        <sup v-if="author.mark">{{ author.mark }}</sup>
      </div>
    </div>

    <div class="affiliation-list">
      <div class="affiliation" v-for="affiliation in config.affiliations">
        <sup v-if="affiliation.mark">{{ affiliation.mark }}</sup>{{ affiliation.text }}
      </div>
    </div>

    <div class="link-list">
      <div class="link" v-for="link in config.links">
        <a :href="link.url" target="_blank">{{ link.text }}</a>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>

