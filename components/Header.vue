<script setup>
import { reactive } from 'vue'

const config = reactive({
  title: 'MeDM<div>Mediating Image Diffusion Models for Video-to-Video Translation with Temporal Correspondence Guidance</div>',
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
  <div class="header">
    <div class="title" v-html="config.title"></div>

    <div class="author-list">
      <div v-for="author in config.authors">
        <a :href="author.homepage" v-if="author.homepage" target="_blank">{{ author.text }}</a>
        <template v-else>{{ author.text }}</template>
        <sup v-if="author.mark">{{ author.mark }}</sup>
      </div>
    </div>

    <div class="affiliation-list">
      <div v-for="affiliation in config.affiliations">
        <sup v-if="affiliation.mark">{{ affiliation.mark }}</sup>{{ affiliation.text }}
      </div>
    </div>

    <div class="link-list">
      <div v-for="link in config.links">
        <a :href="link.url" target="_blank">{{ link.text }}</a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.header {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2em
}
.title {
  font-weight: 500;
  font-size: 2.6rem;
  text-align: center;
  margin-bottom: 30px;
}
.author-list, .affiliation-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  column-gap: 30px;
  font-size: 1.5rem;
  text-align: center;
}
.author-list > div {
  white-space: nowrap;
}

@media (max-width: 814px) {
  .title :deep(div) {
    font-size: 1.2rem;
  }
  .author-list, .affiliation-list {
    font-size: 1.2rem;
  }
}
@media (min-width: 815px) {
  .title :deep(div) {
    display: inline;
    font-weight: inherit;
  }
  .title :deep(div:before) {
    content: ": ";
    font-weight: inherit;
  }
}

.link-list {
  margin-top: 30px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  column-gap: 30px;
  font-size: 1.7rem;
}
.link-list > div {
  background-color: var(--color-background-accent);
}
.link-list > div > a {
  color: var(--color-text-accent);
}
.link-list > div > a:after {
  all: initial;
}
</style>

