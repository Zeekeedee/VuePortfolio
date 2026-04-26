<template>
  <div class="c-stack-content">
    <div v-for="category in categories" :key="category.title" class="c-stack-category">
      <h4 class="c-stack-category__title">{{ category.title }}</h4>
      <ul class="c-stack-category__items">
        <li v-for="item in category.items" :key="item.name" class="c-stack-category__item">
          <Icon :icon="item.icon" class="c-stack-category__icon" />
          {{ item.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue'

const categories = [
  {
    title: 'Programming Languages',
    items: [
      { name: 'C#', icon: 'simple-icons:csharp' },
      { name: 'JavaScript', icon: 'simple-icons:javascript' },
      { name: 'PHP', icon: 'simple-icons:php' },
      { name: 'Python', icon: 'simple-icons:python' },
      { name: 'Java', icon: 'simple-icons:openjdk' },
      { name: 'HTML', icon: 'simple-icons:html5' },
      { name: 'CSS', icon: 'simple-icons:css3' },
      { name: 'Swift', icon: 'simple-icons:swift' },
    ],
  },
  {
    title: 'Frameworks & Libraries',
    items: [
      { name: 'ASP.NET', icon: 'simple-icons:dotnet' },
      { name: 'Vue.js', icon: 'simple-icons:vuedotjs' },
      { name: 'Bootstrap', icon: 'simple-icons:bootstrap' },
      { name: 'Android SDK', icon: 'simple-icons:android' },
    ],
  },
  {
    title: 'Tools & Platforms',
    items: [
      { name: 'Visual Studio', icon: 'simple-icons:visualstudio' },
      { name: 'VS Code', icon: 'simple-icons:visualstudiocode' },
      { name: 'Git', icon: 'simple-icons:git' },
      { name: 'GitHub', icon: 'simple-icons:github' },
      { name: 'WordPress', icon: 'simple-icons:wordpress' },
      { name: 'Figma', icon: 'simple-icons:figma' },
      { name: 'Postman', icon: 'simple-icons:postman' },
    ],
  },
  {
    title: 'Database & Backend',
    items: [
      { name: 'SQL Server', icon: 'simple-icons:microsoftsqlserver' },
      { name: 'REST API', icon: 'mdi:api' },
    ],
  },
  {
    title: 'Other Skills',
    items: [
      { name: 'SEO', icon: 'mdi:magnify' },
      { name: 'UI/UX Design', icon: 'mdi:palette' },
      { name: 'Responsive Design', icon: 'mdi:responsive' },
    ],
  },
]
</script>
