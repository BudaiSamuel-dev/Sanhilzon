<script setup>
import { ref } from 'vue'

// Reaktív változó az aktuális évszám dinamikus megjelenítéséhez
const currentYear = ref(new Date().getFullYear())
</script>

<template>
  <footer class="footer-container py-5 mt-5">
    <div class="container px-4">
      <div class="row g-4 text-start">
        
        <div class="col-12 col-md-4">
          <div class="d-flex align-items-center mb-3">
            <img src="../assets/brand/Logo.png" alt="Sanhilzon Logo" class="footer-logo me-2">
            <span class="footer-brand-text">SANHILZON</span>
          </div>
          <p class="mall">
            Gyakorlatorientált programozási kurzusok és mentorprogramok. Építsük együtt a jövő szoftvereit.
          </p>
        </div>

        <div class="col-12 col-md-4">
          <h5 class="fw-bold mb-3 text-uppercase fs-6 tracking-wider">Navigáció</h5>
          <ul class="list-unstyled footer-links">
            <li class="mb-2"><a href="#hero">Főoldal</a></li>
            <li class="mb-2"><a href="#courses">Kurzusok</a></li>
            <li class="mb-2"><a href="#join-us">Csatlakozás</a></li>
          </ul>
        </div>

        <div class="col-12 col-md-4">
          <h5 class="fw-bold mb-3 text-uppercase fs-6 tracking-wider">Kapcsolat</h5>
          <ul class="list-unstyled text-muted small">
            <li class="mb-2">📍 Budapest, Magyarország</li>
            <li class="mb-2">✉️ info@sanhilzon.hu</li>
            <li class="mb-2">📞 +36 1 234 5678</li>
          </ul>
        </div>

      </div>

      <div class="row mt-5 pt-3 border-top border-secondary-subtle text-center">
        <div class="col-12">
          <p class="text-muted small mb-0">
            © {{ currentYear }} Sanhilzon. Minden jog fenntartva. | Iskolaprojekt
          </p>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
    .footer-container {
    /* A global.css-ben definiált kártya-hátteret használjuk elválasztásként */
    background-color: var(--bg-card); 
    border-top: 1px solid var(--border-color);
    transition: background-color var(--transition-speed), border-color var(--transition-speed);
    }

    .footer-logo {
    height: 32px;
    width: auto;
    }

    .footer-brand-text {
    font-family: 'Montserrat', sans-serif;
    font-weight: 800;
    letter-spacing: 1px;
    color: var(--text-main);
    }

    .tracking-wider {
    letter-spacing: 1px;
    color: var(--text-main);
    }

    /* Gyorslinkek stílusai és hover effektusa */
    .footer-links a {
    color: var(--text-muted);
    text-decoration: none;
    font-size: 0.9rem;
    transition: color var(--transition-speed) ease;
    }

    .footer-links a:hover {
    color: var(--sh-yellow);
    }

    footer p {
    color: var(--text-muted);
    }
</style>