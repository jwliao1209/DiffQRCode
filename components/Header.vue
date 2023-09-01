<script setup>
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
    { text: 'Paper', url: '/medm.pdf', icon: ['fa-regular', 'fa-file-pdf'] },
    { text: 'arXiv', url: 'https://arxiv.org/abs/2308.10079', icon: ['ai', 'ai-arxiv'] },
    { text: 'Code', url: '/code.html', icon: ['fa-brands', 'fa-github'] },
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
      <a :href="link.url" target="_blank" v-for="link in config.links">
        <div>
          <i :class="link.icon"></i>
          {{ link.text }}
        </div>
      </a>
    </div>
  </div>
</template>

<style scoped>
.header {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem
}
.title {
  font-weight: 500;
  font-size: 2.1rem;
  text-align: center;
  margin-bottom: 2rem;
}
.author-list, .affiliation-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  column-gap: 1.5rem;
  font-size: 1.2rem;
  text-align: center;
}
.author-list > div {
  white-space: nowrap;
}

@media (max-width: 850px) {
  .title :deep(div) {
    font-size: 1.2rem;
    color: var(--color-text-mute);
  }
}
@media (min-width: 850px) {
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
  margin-top: 2rem;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem 2rem;
  font-size: 1.2rem;
}
.link-list > a:after {
  all: initial;
}
.link-list > a > div {
  background-color: var(--color-background-accent);
  padding: 0.2rem 1.3rem;
  border-radius: 100px;
  color: var(--color-text-accent);
  transition: transform .2s;
}
@media (hover: hover) {
  .link-list > a > div:hover {
    transform: scale(1.05);
  }
}
.link-list > a > div:active {
  transform: scale(1.05);
}
@media (prefers-color-scheme: dark) {
  .link-list > a > div {
    border: 1px solid white;
  }
}
i {
  margin-right: 0.1rem;
}
</style>

