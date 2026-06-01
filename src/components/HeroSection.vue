<script setup></script>

<template>
    <section id="hero" class="hero-container d-flex align-items-center justify-content-center text-center">
    
    <video autoplay muted loop playsinline class="hero-video">
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
        height: 100vh; /* Teljes képernyőmagasság */
        overflow: hidden;
        background-color: var(--sh-dark); /* Tartalék háttérszín, amíg a videó töltődik */
    }

    /* 1. Réteg: A Videó elhelyezése */
    .hero-video {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover; /* Kitölti a teret torzulás nélkül */
        z-index: 1;
    }

    /* 2. Réteg: A Sötétítő Overlay */
    .hero-overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(18, 18, 18, 0.65); /* 65%-os sötétítés az olvashatóságért */
        z-index: 2;
    }

    /* 3. Réteg: A Tartalom */
    .hero-content {
        position: relative;
        z-index: 3; /* A videó és az overlay felett helyezkedik el */
        color: #FFFFFF; /* A fejléc szövege mindig fehér marad, függetlenül a sötét módtól */
    }

    .main-title {
        font-weight: 800;
        letter-spacing: -1px;
    }

    /* Egy kis extra modern dizájn: színátmenetes kiemelés a szövegben */
    .text-gradient {
        color: var(--sh-yellow);
    }

    .sub-title {
        max-width: 600px;
        font-weight: 400;
        color: rgba(255, 255, 255, 0.85);
    }

    /* Reszponzív finomítások kisebb kijelzőkre (Mobilra) */
    @media (max-width: 767.98px) {
        .main-title {
            font-size: 2.5rem; /* Kisebb betűméret mobilon, hogy ne törjön csúnyán */
        }
        .sub-title {
            font-size: 1rem;
        }
    }
</style>