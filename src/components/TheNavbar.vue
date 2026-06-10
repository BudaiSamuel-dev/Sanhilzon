<script setup>
    import { ref, onMounted, onUnmounted } from 'vue'

     defineProps({
        isDark: Boolean
    })

    defineEmits(['toggle-theme'])
    const isScrolled = ref(false)
    const isNavHidden = ref(false)
    const isMobileMenuOpen = ref(false)
    let lastScrollPosition = 0

     const toggleMobileMenu = () => {
    isMobileMenuOpen.value = !isMobileMenuOpen.value
    }

    const closeMobileMenu = () => {
    isMobileMenuOpen.value = false
    }
    const handleScroll = () => {
      const currentScrollPosition = window.pageYOffset || document.documentElement.scrollTop
      
      if (currentScrollPosition < 0) return

      isScrolled.value = currentScrollPosition > 50

      if (currentScrollPosition > lastScrollPosition && currentScrollPosition > 150) {
          isNavHidden.value = true
      } else {
          isNavHidden.value = false
      }
      
      lastScrollPosition = currentScrollPosition
    }

    onMounted(() => {
    window.addEventListener('scroll', handleScroll)
    })

    onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll)
    })
</script>

<template>
  <nav 
    class="navbar navbar-expand-lg fixed-top px-3 px-md-5 transition-nav"
    :class="{ 'scrolled': isScrolled, 'nav-hidden': isNavHidden }"
  >
    <div class="container-fluid">
      <a class="navbar-brand d-flex align-items-center" href="#">
        <img src="../assets/brand/Logo.png" alt="Sanhilzon Logo" class="logo-img me-2">
        <span class="brand-text">SANHILZON</span>
      </a>

      <button 
        class="navbar-toggler custom-toggler" 
        type="button" 
        @click="toggleMobileMenu"
        aria-controls="navbarNav" 
        aria-expanded="false" 
        aria-label="Navigáció váltása"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" :class="{ 'show': isMobileMenuOpen }" id="navbarNav">
        <ul class="navbar-nav ms-auto align-items-center">
          <li class="nav-item">
            <a class="nav-link" href="#hero" @click="closeMobileMenu">Főoldal</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#courses" @click="closeMobileMenu">Kurzusok</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#join-us" @click="closeMobileMenu">Csatlakozás</a>
          </li>
          
          <li class="nav-item ms-lg-3 mt-3 mt-lg-0">
            <button @click="$emit('toggle-theme')" class="btn-theme-toggle" aria-label="Téma váltása">
              <span v-if="isDark">☀️</span>
              <span v-else>🌙</span>
            </button>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<style scoped>
    .transition-nav {
        transition: transform 0.3s ease, background-color 0.3s ease, box-shadow 0.3s ease;
        background-color: transparent;
        padding-top: 1.5rem;
        padding-bottom: 1.5rem;
    }
   .scrolled {
        background-color: var(--nav-bg);
        backdrop-filter: blur(10px); /* Modern elmosódott háttér effekt */
        padding-top: 0.8rem;
        padding-bottom: 0.8rem;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
    }

     .nav-hidden {
        transform: translateY(-100%);
    }

    .logo-img {
        height: 40px;
        width: auto;
    }

    .brand-text {
        font-family: 'Montserrat', sans-serif;
        font-weight: 800;
        letter-spacing: 1px;
        color: #ffff;
    }

    .scrolled .brand-text {
        color: var(--nav-text) !important;
    }

    .navbar-nav .nav-link {
        color: rgba(255, 255, 255, 0.9);
        font-weight: 500;
        margin: 0 0.5rem;
        transition: color var(--transition-speed);
    }

    .scrolled .navbar-nav .nav-link {
        color: var(--nav-text) !important;
    }

    .nav-link:hover {
        color: var(--sh-yellow) !important;
    }

    .btn-theme-toggle {
        background: var(--bg-card);
        border: 1px solid var(--border-color);
        font-size: 1.2rem;
        padding: 0.4rem 0.8rem;
        border-radius: 50%;
        cursor: pointer;
        transition: transform 0.2s, background-color var(--transition-speed);
    }

    .btn-theme-toggle:hover {
        transform: scale(1.1);
    }

    /* Mobilos reszponzív finomítás a lenyíló menü hátteréhez */
    @media (max-width: 991.98px) {
        .navbar-collapse {
            background-color: var(--bg-card);
            padding: 1rem;
            border-radius: 8px;
            margin-top: 1rem;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
    }
</style>