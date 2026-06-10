<script setup>
    
    defineProps({
    title: String,
    description: String,
    duration: String,
    level: String
    })
</script>

<template>
  <div class="card h-100 shadow-sm course-card">
    <div class="card-body d-flex flex-column justify-content-between p-4">
      <div>
        <span class="badge bg-secondary mb-2 fs-7 text-uppercase">{{ level }}</span>
        <h3 class="card-title h4 my-2">{{ title }}</h3>
        <p class="card-text text-muted fs-6">{{ description }}</p>
      </div>
      
      <div class="mt-4 pt-3 border-top d-flex justify-content-between align-items-center">
        <span class="text-muted fs-7">⏱️ {{ duration }}</span>
        <a href="#join-us" class="btn btn-sanhilzon btn-sm">Jelentkezem</a>
      </div>
    </div>
  </div>
</template>

<style scoped>
   
    .course-card:hover {
        transform: translateY(-5px);
        box-shadow: 0 8px 25px rgba(255, 204, 0, 0.15) !important;
        border-color: var(--sh-yellow) !important;
    }

    .fs-7 {
        font-size: 0.85rem;
    }

    .text-muted {
        color: var(--text-muted) !important;
    }
</style>