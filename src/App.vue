<script setup>
  import { ref, onMounted } from 'vue'
  import TheNavbar from './components/TheNavbar.vue'
  import HeroSection from './components/HeroSection.vue'
  import CourseSection from './components/CourseSection.vue'
  import JoinSection from './components/JoinSection.vue'

  // Reaktív változó a sötét módhoz (alapértelmezetten hamis, azaz világos mód)
  const isDarkMode = ref(false)

  // Függvény a téma kapcsolásához
  const toggleTheme = () => {
    isDarkMode.value = !isDarkMode.value
    // Elmentjük a választást a böngészőben, hogy frissítés után se vesszen el
    localStorage.setItem('theme', isDarkMode.value ? 'dark' : 'light')
    updateBodyClass()
  }

  // Mivel a body elem a Vue hatáskörén kívül van, közvetlenül adjuk hozzá a CSS osztályt
  const updateBodyClass = () => {
    if (isDarkMode.value) {
      document.body.classList.add('dark-theme')
    } else {
      document.body.classList.remove('dark-theme')
    }
  }

  // Amikor a főkomponens betöltődik, megnézzük, mit választott legutóbb a felhasználó
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
