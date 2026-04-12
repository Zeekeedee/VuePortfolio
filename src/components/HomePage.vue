<template>
  <div class="c-header">
    <div class="c-header__info">
      <h1 class="c-header__title">./zeke/dev/portfolio_</h1>
      <span class="c-header__version">Build: v1.0.0 - Theme: GMK Oblivion</span>
    </div>
    <span class="c-header__status">Status: Open to work</span>
  </div>
  <div class="c-homepage" :class="{ 'has-active': activeId }">
    <section
      v-for="s in sections"
      :key="s.id"
      :class="[s.class, { [`${s.class}--expanded`]: activeId === s.id }]"
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
import { ref } from 'vue'
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

const componentMap: Record<string, Component> = {
  experience: Experience,
  about: AboutMe,
  contact: Contact,
  stack: TechStack,
}

const sections = ref<Section[]>([
  { id: 'hero', title: "Hi, I'm Zeke ->", class: 'c-hero' },
  { id: 'about', title: '01. About Me', content: 'Mid-level Software Developer with over 6 years of experience at Miller Development Company, specializing in front-end development and UI/UX improvement.', class: 'c-about' },
  { id: 'stack', title: '02. Stack', class: 'c-stack', showContent: true },
  { id: 'projects', title: 'Github', class: 'c-projects' },
  { id: 'experience', title: 'Experience', class: 'c-experience' },
  { id: 'skills', title: 'Skills', class: 'c-skills' },
  { id: 'contact', title: '03. Contact', content: 'Start a conversation', class: 'c-contact' },
])

const handleSectionClick = (section: Section) => {
  if (section.id === 'projects') {
    window.open('https://github.com/Zeekeedee', '_blank')
  } else {
    toggleSection(section.id)
  }
}

const toggleSection = (id: string) => {
  activeId.value = activeId.value === id ? null : id
}
</script>
