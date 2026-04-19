<template>
  <div class="c-header">
    <div class="c-header__info">
      <h1 class="c-header__title">./zeke/dev/portfolio_</h1>
      <span class="c-header__version">Build: v1.0.0 - Theme: GMK Oblivion</span>
    </div>
    <span class="c-header__status">Status: Open to work</span>
  </div>
  <div
    class="c-homepage"
    :class="[{ 'has-active': activeId }, activeId ? `active-${activeId}` : '']"
  >
    <section
      v-for="s in sections"
      :key="s.id"
      :class="[s.class, { [`${s.class}--expanded`]: activeId === s.id }]"
      @click="handleSectionClick(s)"
    >
      <div>
        <h3 class="c-section-title">{{ s.title }}</h3>
        <div class="c-section-content">
          <p v-if="activeId !== s.id && !s.showContent">{{ s.content }}</p>
          <component v-if="activeId === s.id || s.showContent" :is="componentMap[s.id]" />
        </div>
      </div>
    </section>
  </div>
  <div class="c-footer">
    <p>&copy; 2026 DESIGNED_BY_ZEKE. ALL_RIGHTS_RESERVED.</p>
    <div class="c-footer__links">
      <a href="https://github.com/Zeekeedee" target="_blank" rel="noopener noreferrer">GitHub</a>
      <a href="#">LinkedIn</a>
      <a href="#">Twitter</a>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import type { Component } from 'vue'
import Experience from './Experience.vue'
import AboutMe from './AboutMe.vue'
import Contact from './Contact.vue'
import TechStack from './TechStack.vue'
import { wrapGrid } from 'animate-css-grid'
import { nextTick } from 'vue'

interface Section {
  id: string
  title: string
  class: string
  content?: string
  showContent?: boolean
}

const activeId = ref<string | null>(null)

const componentMap: Record<string, Component> = {
  experience: Experience,
  about: AboutMe,
  contact: Contact,
  stack: TechStack,
}

const sections = ref<Section[]>([
  { id: 'hero', title: "Hi, I'm Zeke ->", class: 'c-hero' },
  {
    id: 'about',
    title: '01. About Me',
    content:
      'Mid-level Software Developer with over 6 years of experience at Miller Development Company, specializing in front-end development and UI/UX improvement.',
    class: 'c-about',
  },
  { id: 'stack', title: '02. Stack', class: 'c-stack', showContent: true },
  { id: 'projects', title: 'Github', class: 'c-projects' },
  { id: 'experience', title: 'Experience', class: 'c-experience' },
  { id: 'skills', title: 'Skills', class: 'c-skills' },
  { id: 'contact', title: '03. Contact', content: 'Start a conversation', class: 'c-contact' },
])

// const handleSectionClick = (section: Section) => {
//   if (section.id === 'projects') {
//     window.open('https://github.com/Zeekeedee', '_blank')
//   } else {
//     toggleSection(section.id)
//   }
// }

// const toggleSection = (id: string) => {
//   activeId.value = activeId.value === id ? null : id
// }

const animatedGrid = {
  duration: 400,
  onStart: (elements: Element[]) =>
    console.log(`started animation for ${elements.length} elements`),
  onEnd: (elements: Element[]) => console.log(`finished animation for ${elements.length} elements`),
  stagger: 0,
  easing: 'easeInOut' as const,
  exclude: '*:not(.c-homepage > section)',
}


// ... your existing setup

let unwrapGrid: (() => void) | null = null

onMounted(() => {
  const grid = document.querySelector(".c-homepage") as HTMLElement
  if (grid) {
    const { unwrap } = wrapGrid(grid, animatedGrid)
    unwrapGrid = unwrap
  }
})

const handleSectionClick = async (s) => {
  // Update state first
  activeId.value = activeId.value === s.id ? null : s.id

  // Flush Vue's DOM updates BEFORE the grid captures new positions
  await nextTick()

  // Force the grid to re-measure after DOM is stable
  const grid = document.querySelector(".c-homepage") as HTMLElement
  if (grid && unwrapGrid) {
    unwrapGrid()
    const { unwrap } = wrapGrid(grid, animatedGrid)
    unwrapGrid = unwrap
  }
}
</script>
