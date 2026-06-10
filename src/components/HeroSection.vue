<script setup></script>

<template>
    <section id="hero" class="hero-container d-flex align-items-center justify-content-center text-center">
    
    <video autoplay muted loop playsinline aria-hidden="true" class="hero-video">
      <source src="../assets/videos/hero-bg.mp4" type="video/mp4" />
      A böngésződ nem támogatja a videó lejátszását.
    </video>

    <div class="hero-overlay"></div>

    <div class="hero-content container px-4">
      <h1 class="display-3 main-title mb-3">
        Lépj be a jövő <span class="text-gradient">fejlesztői</span> közé
      </h1>
      <p class="lead sub-title mb-4 mx-auto">
        A Sanhilzon csapatával nem csak elméletet tanulsz. Valódi projekteken, 
        modern technológiákkal építjük fel a karriered.
      </p>
      <a href="#courses" class="btn btn-sanhilzon btn-lg px-5 py-3 fs-5">
        Kurzusok felfedezése
      </a>
    </div>
  </section>
</template>

<style scoped>
    .hero-container {
        position: relative;
        width: 100%;
        height: 100vh; 
        overflow: hidden;
        background-color: var(--sh-dark); 
    }

    
    .hero-video {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover; 
        z-index: 1;
    }

    
    .hero-overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(18, 18, 18, 0.65); 
        z-index: 2;
    }

    /* 3. Réteg: A Tartalom */
    .hero-content {
        position: relative;
        z-index: 3; 
        color: #FFFFFF; 
    }

    .main-title {
        font-weight: 800;
        letter-spacing: -1px;
    }

    .text-gradient {
        color: var(--sh-yellow);
    }

    .sub-title {
        max-width: 600px;
        font-weight: 400;
        color: rgba(255, 255, 255, 0.85);
    }

    
    @media (max-width: 767.98px) {
        .main-title {
            font-size: 2.5rem; 
        }
        .sub-title {
            font-size: 1rem;
        }
    }
</style>