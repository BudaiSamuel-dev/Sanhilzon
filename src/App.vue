<script setup>
  import { ref, onMounted } from 'vue'
  import TheNavbar from './components/TheNavbar.vue'
  import HeroSection from './components/HeroSection.vue'
  import CourseSection from './components/CourseSection.vue'
  import JoinSection from './components/JoinSection.vue'
  import TheFooter from './components/TheFooter.vue'

  const isDarkMode = ref(false)

  const toggleTheme = () => {
    isDarkMode.value = !isDarkMode.value
    localStorage.setItem('theme', isDarkMode.value ? 'dark' : 'light')
    updateBodyClass()
  }

  const updateBodyClass = () => {
    if (isDarkMode.value) {
      document.body.classList.add('dark-theme')
    } else {
      document.body.classList.remove('dark-theme')
    }
  }

  onMounted(() => {
    const savedTheme = localStorage.getItem('theme')
    if (savedTheme === 'dark') {
      isDarkMode.value = true
      updateBodyClass()
    }
  })
</script>

<template>
  <div :class="{ 'dark-theme': isDarkMode }" class="app-container">
    
    <TheNavbar :is-dark="isDarkMode" @toggle-theme="toggleTheme" />

    
    <main>
      <HeroSection />
      <CourseSection />
      <JoinSection />
    </main>

    <TheFooter />
  </div>
</template>

<style scoped>
  .app-container {
    min-height: 100vh;
    background-color: var(--bg-main);
    color: var(--text-main);
    transition: background-color var(--transition-speed), color var(--transition-speed);
  }
</style>
