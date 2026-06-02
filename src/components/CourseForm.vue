<script setup>
    import { ref } from 'vue'

    // 1. A szülőtől megkapjuk a kurzusok listáját, hogy dinamikusan jeleníthessük meg a választót
    defineProps({
    courses: Array
    })

    const emit = defineEmits(['submit-enrollment'])

    // 2. Az adatmodell pontosan követi a db.json "enrollments" mezőit
    const formData = ref({
        courseId: '',
        studentName: '',
        email: '',
        phone: ''
    })

    const handleSubmit = () => {
        // Kliensoldali validálás
        if (!formData.value.courseId || !formData.value.studentName || !formData.value.email || !formData.value.phone) {
            Swal.fire({
            icon: 'error',
            title: 'Hiányzó adatok!',
            text: 'Kérjük, töltsd ki az összes kötelező mezőt a jelentkezéshez.',
            confirmButtonColor: '#FFCC00',
            background: 'var(--bg-main)',
            color: 'var(--text-main)'
            })
            return
        }

        // Ha minden ki van töltve, az adatokat egy objektumba csomagolva felküldjük a szülőnek
        emit('submit-enrollment', { ...formData.value })
    }

    // Űrlap ürítése sikeres AJAX POST után
    const resetForm = () => {
        formData.value.courseId = ''
        formData.value.studentName = ''
        formData.value.email = ''
        formData.value.phone = ''
    }

    defineExpose({ resetForm })
</script>

<template>
  <form @submit.prevent="handleSubmit" class="p-3">
    <div class="mb-3 text-start">
      <label for="studentName" class="form-label fw-bold">Teljes név *</label>
      <input type="text" v-model="formData.studentName" class="form-control" id="studentName" placeholder="Nagy Ádám">
    </div>
    
    <div class="mb-3 text-start">
      <label for="studentEmail" class="form-label fw-bold">E-mail cím *</label>
      <input type="email" v-model="formData.email" class="form-control" id="studentEmail" placeholder="adam.nagy@email.com">
    </div>

    <div class="mb-3 text-start">
      <label for="studentPhone" class="form-label fw-bold">Telefonszám *</label>
      <input type="text" v-model="formData.phone" class="form-control" id="studentPhone" placeholder="+36301234567">
    </div>

    <div class="mb-4 text-start">
      <label for="courseSelect" class="form-label fw-bold">Válaszd ki a kurzust *</label>
      <select v-model="formData.courseId" class="form-select" id="courseSelect">
        <option value="" disabled>Válassz kurzust...</option>
        <option v-for="course in courses" :key="course.id" :value="course.id">
          {{ course.title }}
        </option>
      </select>
    </div>

    <button type="submit" class="btn btn-sanhilzon w-100 py-2">Jelentkezés beküldése</button>
  </form>
</template>

<style scoped>
    #studentName, #studentEmail, #studentPhone, #courseSelect {
        background-color: var(--bg-main) !important;
        color: var(--text-main) !important;
        border: 1px solid var(--border-color) !important;
        transition: background-color var(--transition-speed), color var(--transition-speed), border-color var(--transition-speed);
    }
</style>