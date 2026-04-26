<template>
  <div class="c-header">
    <div class="c-header__info">
      <h1 class="c-header__title">./zeke/dev/portfolio_</h1>
      <span class="c-header__version">Build: v1.0.0 - Theme: GMK Oblivion</span>
    </div>
    <span class="c-header__status">Status: Open to work</span>
  </div>
  <div class="c-homepage" :class="{ 'has-active': activeId, [`active-${activeId}`]: !!activeId }">
    <section
      v-for="s in sections"
      :key="s.id"
      :class="[s.class, { [`${s.class}--expanded`]: activeId === s.id }]"
      ref="items"
      @click="handleSectionClick(s)"
    >
      <h3 class="c-section-title">{{ s.title }}</h3>
      <div class="c-section-content">
        <p v-if="activeId !== s.id && !s.showContent">{{ s.content }}</p>
        <component v-if="activeId === s.id || s.showContent" :is="componentMap[s.id]" />
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
import { ref, nextTick } from 'vue'
import type { Component } from 'vue'
import Experience from './Experience.vue'
import AboutMe from './AboutMe.vue'
import Contact from './Contact.vue'
import TechStack from './TechStack.vue'

interface Section {
  id: string
  title: string
  class: string
  content?: string
  showContent?: boolean
}

const activeId = ref<string | null>(null)
const items = ref<HTMLElement[]>([])

const componentMap: Record<string, Component> = {
  experience: Experience,
  about: AboutMe,
  contact: Contact,
  stack: TechStack,
}

const sections = ref<Section[]>([
  { id: 'hero', title: "Hi, I'm Zeke ->", class: 'c-hero' },
  { id: 'about', title: '01. About Me', content: 'A quick look at who I am and what I build', class: 'c-about' },
  { id: 'stack', title: '02. Stack', class: 'c-stack', showContent: true },
  { id: 'projects', title: 'Github', class: 'c-projects' },
  { id: 'experience', title: 'Experience', class: 'c-experience' },
  { id: 'skills', title: 'Skills', class: 'c-skills' },
  { id: 'contact', title: '03. Contact', content: 'Start a conversation', class: 'c-contact' },
])

const EXPANDABLE = new Set(['about', 'stack', 'contact'])

const handleSectionClick = (section: Section) => {
  if (section.id === 'projects') {
    window.open('https://github.com/Zeekeedee', '_blank')
  } else if (EXPANDABLE.has(section.id)) {
    toggleSection(section.id)
  }
}

function animateFlip(elements: HTMLElement[], firstRects: DOMRect[]) {
  elements.forEach((el, i) => {
    const first = firstRects[i]
    const last = el.getBoundingClientRect()
    const dx = first.left - last.left
    const dy = first.top - last.top
    const scaleX = first.width / last.width
    const scaleY = first.height / last.height

    if (Math.abs(dx) < 0.5 && Math.abs(dy) < 0.5 && Math.abs(scaleX - 1) < 0.005 && Math.abs(scaleY - 1) < 0.005) return

    // Expanding items wait for siblings to clear the space first.
    // fill: 'backwards' holds the first keyframe during the delay so the
    // item stays visually pinned at its old (smaller) position until it starts.
    const isExpanding = last.width > first.width || last.height > first.height
    const delay = isExpanding ? 100 : 0

    el.getAnimations().forEach((a) => a.cancel())
    el.animate(
      [
        { transformOrigin: 'top left', transform: `translate(${dx}px, ${dy}px) scale(${scaleX}, ${scaleY})` },
        { transformOrigin: 'top left', transform: 'none' },
      ],
      { duration: 200, delay, easing: 'cubic-bezier(0.4, 0, 0.2, 1)', fill: isExpanding ? 'backwards' : 'none' },
    )
  })
}

const toggleSection = async (id: string) => {
  const elements = [...items.value]
  const firstRects = elements.map((el) => el.getBoundingClientRect())

  activeId.value = activeId.value === id ? null : id

  await nextTick()
  requestAnimationFrame(() => animateFlip(elements, firstRects))
}
</script>
