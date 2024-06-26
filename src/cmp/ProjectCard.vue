<script setup>
import { toTitleCase } from '@/script/string.js'
import { computed, ref } from 'vue'
const props = defineProps(['name', 'route', 'tags', 'href', 'site', 'img', 'size'])
const showScreenshot = ref(true)
const action = computed(() => {
  return props.tags?.includes('Code-Only')
    ? 'View Source'
    : props.tags?.includes('game') || props.tags?.includes('game jam')
      ? 'Play'
      : 'View'
})
const href = computed(
  () =>
    props.href ??
    (props.tags?.includes('Code-Only')
      ? `https://github.com/espoire/${props.route}`
      : `https://espoire.github.io/${props.route}`)
)
const site = computed(
  () => props.site ?? (props.tags?.includes('Code-Only') ? 'GitHub' : 'GitHub Pages')
)
function hide() {
  showScreenshot.value = false
}
</script>

<template>
  <div class="card" :style="showScreenshot ? `min-height: calc(min(30dvw, 40dvh${props.size ? `, ${props.size}px` : ''}) + 8rem);` : ''">
    <div class="header">
      <div class="nameAndTagsWrap">
        <h2 class="name">{{ props.name }}</h2>
        <div class="tags">
          <span v-for="(tag, i) of props.tags" :key="i">{{ toTitleCase(tag) }}</span>
        </div>
      </div>
      <div v-if="props.route">
        <a :href="href"> {{ action }} on {{ site }} </a>
      </div>
    </div>

    <img
      v-if="showScreenshot"
      class="screenshot"
      :style="props.size ? `max-width: min(25dvw, ${props.size}px); max-height: min(30dvw, 40dvh, ${props.size}px));` : ''"
      :src="props.img ?? `./${props.route}.gif`"
      @error="hide"
      :title="`Screenshot from ${props.name}`"
    />
    <p class="description"><slot></slot></p>
  </div>
</template>

<style lang="scss" scoped>
.card {
  background-color: var(--color-background-soft);
  padding: 2rem;
  border-radius: 2rem;
  margin-top: 2rem;

  @media (max-width: 26rem) {
    padding: 1rem;
  }
}

.header {
  display: flex;
  justify-content: space-between;
}

.nameAndTagsWrap {
  display: flex;
  justify-content: space-between;

  @media (max-width: 70rem) {
    flex-direction: column;
  }
}

.name {
  line-height: 1;
  margin-right: 1rem;
}

.tags {
  span {
    display: inline-block;
    background-color: var(--color-background-mute);
    padding: 0.5rem;
    border-radius: 50cqh;
    margin-left: 0.5rem;
    line-height: 1;
  }
}

.screenshot {
  float: right;
  max-width: 25dvw;
  max-height: min(30dvw, 40dvh);
  margin-left: 2rem;
  margin-bottom: 2rem;
  margin-top: 2rem;
  border: 0.2rem solid var(--color-border);
  border-radius: 0.4rem;
}

.description {
  margin-top: 2rem;
  text-align: justify;
}
</style>
