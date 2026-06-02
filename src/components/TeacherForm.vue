<script setup>
    import { ref } from 'vue'

    defineProps({
        categories: Array
    })

    const emit = defineEmits(['submit-application'])

    // Az adatmodell pontosan követi a db.json "applications" sémáját
    const formData = ref({
        fullName: '',
        email: '',
        phone: '',
        role: '',
        targetCategory: '',
        experienceYears: null,
        linkedinProfile: '',
        introduction: ''
    })

    const handleSubmit = () => {
    // LinkedIn profil opcionális lehet, a többi kötelező
    if (!formData.value.fullName || !formData.value.email || !formData.value.phone || 
        !formData.value.role || !formData.value.targetCategory || !formData.value.experienceYears || !formData.value.introduction) {
        Swal.fire({
        icon: 'error',
        title: 'Hiányzó adatok!',
        text: 'Kérjük, töltsd ki az összes kötelező mezőt a csatlakozáshoz.',
        confirmButtonColor: '#FFCC00',
        background: 'var(--bg-main)',
        color: 'var(--text-main)'
        })
        return
    }

    emit('submit-application', { ...formData.value })
    }

    const resetForm = () => {
        formData.value.fullName = ''
        formData.value.email = ''
        formData.value.phone = ''
        formData.value.role = ''
        formData.value.targetCategory = ''
        formData.value.experienceYears = null
        formData.value.linkedinProfile = ''
        formData.value.introduction = ''
    }

    defineExpose({ resetForm })
</script>

<template>
  <form @submit.prevent="handleSubmit" class="p-3">
    <div class="row">
      <div class="col-md-6 mb-3 text-start">
        <label for="fullName" class="form-label fw-bold">Teljes név *</label>
        <input type="text" v-model="formData.fullName" class="form-control" id="fullName" placeholder="Tóth Gábor">
      </div>
      <div class="col-md-6 mb-3 text-start">
        <label for="teacherPhone" class="form-label fw-bold">Telefonszám *</label>
        <input type="text" v-model="formData.phone" class="form-control" id="teacherPhone" placeholder="+36209876543">
      </div>
    </div>

    <div class="mb-3 text-start">
      <label for="teacherEmail" class="form-label fw-bold">E-mail cím *</label>
      <input type="email" v-model="formData.email" class="form-control" id="teacherEmail" placeholder="gabor.toth@email.com">
    </div>

    <div class="row">
      <div class="col-md-6 mb-3 text-start">
        <label for="roleSelect" class="form-label fw-bold">Keresett pozíció *</label>
        <select v-model="formData.role" class="form-select" id="roleSelect">
          <option value="" disabled>Válassz...</option>
          <option value="Oktató">Oktató (Kurzusvezető)</option>
          <option value="Mentor">Mentor (Segítő)</option>
        </select>
      </div>
      <div class="col-md-6 mb-3 text-start">
        <label for="categorySelect" class="form-label fw-bold">Szakmai terület *</label>
        <select v-model="formData.targetCategory" class="form-select" id="categorySelect">
          <option value="" disabled>Válassz...</option>
          <option v-for="cat in categories" :key="cat.id" :value="cat.name">
            {{ cat.name }}
          </option>
        </select>
      </div>
    </div>

    <div class="row">
      <div class="col-md-6 mb-3 text-start">
        <label for="expYears" class="form-label fw-bold">Tapasztalat (években) *</label>
        <input type="number" v-model.number="formData.experienceYears" class="form-control" id="expYears" min="0" placeholder="5">
      </div>
      <div class="col-md-6 mb-3 text-start">
        <label for="linkedin" class="form-label fw-bold">LinkedIn profil (opcionális)</label>
        <input type="url" v-model="formData.linkedinProfile" class="form-control" id="linkedin" placeholder="https://linkedin.com/in/...">
      </div>
    </div>

    <div class="mb-4 text-start">
      <label for="intro" class="form-label fw-bold">Rövid bemutatkozás *</label>
      <textarea v-model="formData.introduction" class="form-control" id="intro" rows="3" placeholder="Mesélj magadról és a szakmai hátteredről..."></textarea>
    </div>

    <button type="submit" class="btn btn-sanhilzon w-100 py-2">Jelentkezés elküldése</button>
  </form>
</template>

<style scoped>
    #fullName, #teacherPhone, #teacherEmail, #roleSelect, #categorySelect, #expYears, #linkedin, #intro {
        background-color: var(--bg-main) !important;
        color: var(--text-main) !important;
        border: 1px solid var(--border-color) !important;
        transition: background-color var(--transition-speed), color var(--transition-speed), border-color var(--transition-speed);
    }

    #fullName::placeholder, #teacherPhone::placeholder, #teacherEmail::placeholder, #linkedin::placeholder, #intro::placeholder {
        color: var(--text-muted) !important;
        opacity: 1 !important;
    }

    #roleSelect option, #categorySelect option {
        background-color: var(--bg-main) !important;
        color: var(--text-main) !important;
    }   

</style>