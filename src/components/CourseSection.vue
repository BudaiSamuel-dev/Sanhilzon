<script setup>
    import { ref, onMounted } from 'vue'
    import CourseCard from './CourseCard.vue'

    // Reaktív változók az AJAX állapotok követéséhez
    const courses = ref([])
    const isLoading = ref(true)
    const errorMessage = ref('')

    // Az AJAX GET kérés függvénye tisztán XMLHttpRequest-tel
    const loadCourses = () => {
        const xhr = new XMLHttpRequest()
    
        xhr.open('GET', 'http://localhost:3001/courses', true)
        
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                isLoading.value = false 
                if (xhr.status === 200) {
                    try {
                        courses.value = JSON.parse(xhr.responseText)
                    } catch (e) {
                        errorMessage.value = 'Adatfeldolgozási hiba történt.'
                    }
                } else {
                    errorMessage.value = `Hiba történt az adatok lekérése során. (Státusz: ${xhr.status})`
                }
            }
        }
        xhr.send()
    }

        // Amint a komponens a helyére kerül, azonnal lőjük az AJAX kérést
    onMounted(() => {
        loadCourses()
    })
</script>

<template>
  <section id="courses" class="py-5 section-padding">
    <div class="container px-4">
      
      <div class="text-center mb-5">
        <h2 class="display-5 section-title">Elérhető Kurzusaink</h2>
        <p class="text-muted mx-auto sub-text" style="max-width: 500px;">
          Válogass gyakorlatorientált képzéseink közül és sajátítsd el a szakma fortélyait.
        </p>
      </div>

      <div v-if="isLoading" class="text-center my-5">
        <div class="spinner-border text-warning" role="status" style="width: 3rem; height: 3rem;">
          <span class="visually-hidden">Betöltés...</span>
        </div>
        <p class="text-muted mt-3">Kurzusok betöltése a Sanhilzon adatbázisból...</p>
      </div>

      <div v-else-if="errorMessage" class="alert alert-danger text-center my-5 col-md-6 mx-auto" role="alert">
        <h4 class="alert-heading">⚠️ Hiba történt!</h4>
        <p class="mb-0">{{ errorMessage }}</p>
      </div>

      <div v-else class="row g-4 justify-content-center">
        <div 
          class="col-12 col-md-6 col-lg-4" 
          v-for="course in courses" 
          :key="course.id"
        >
          <CourseCard 
            :title="course.title"
            :description="course.description"
            :duration="course.duration"
            :level="course.level"
          />
        </div>
      </div>

    </div>
  </section>
</template>

<style scoped>
    .section-padding {
    padding-top: 5rem;
    padding-bottom: 5rem;
    }
    .section-title {
    font-weight: 700;
    position: relative;
    display: inline-block;
    padding-bottom: 10px;
    }
    /* Kis sárga díszítő vonal a cím alatt */
    .section-title::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 60px;
    height: 4px;
    background-color: var(--sh-yellow);
    border-radius: 2px;
    }
</style>