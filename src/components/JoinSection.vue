<script setup>
import { ref, onMounted } from 'vue'
import CourseForm from './CourseForm.vue'
import TeacherForm from './TeacherForm.vue'

// 1. Reaktív állapotok
const activeForm = ref('student') // Lehetséges értékek: 'student' vagy 'teacher'
const courses = ref([])
const categories = ref([])

// 2. Referenciák az űrlap-komponensekre, hogy sikeres küldés után kiüríthessük őket
const studentFormRef = ref(null)
const teacherFormRef = ref(null)

// 3. Adatok előzetes lekérése a legördülő menükhöz (XHR GET)
const loadFormData = () => {
  // Kurzusok lekérése
  const xhrCourses = new XMLHttpRequest()
  xhrCourses.open('GET', 'http://localhost:3001/courses', true)
  xhrCourses.onreadystatechange = function () {
    if (xhrCourses.readyState === 4 && xhrCourses.status === 200) {
      courses.value = JSON.parse(xhrCourses.responseText)
    }
  }
  xhrCourses.send()

  // Kategóriák lekérése a tanári formhoz
  const xhrCategories = new XMLHttpRequest()
  xhrCategories.open('GET', 'http://localhost:3001/categories', true)
  xhrCategories.onreadystatechange = function () {
    if (xhrCategories.readyState === 4 && xhrCategories.status === 200) {
      categories.value = JSON.parse(xhrCategories.responseText)
    }
  }
  xhrCategories.send()
}
// DIÁK JELENTKEZÉS BEKÜLDÉSE (XHR POST -> /enrollments)
const handleEnrollmentSubmit = (studentData) => {
  const xhrGet = new XMLHttpRequest()
  xhrGet.open('GET', 'http://localhost:3001/enrollments', true)
  
  xhrGet.onreadystatechange = function () {
    if (xhrGet.readyState === 4 && xhrGet.status === 200) {
      const currentEnrollments = JSON.parse(xhrGet.responseText)
      
      let nextIdNumber = 1
      if (currentEnrollments.length > 0) {
        // Most már a mi egyedi 'customId' mezőnkből olvassuk ki a számokat!
        const validIds = currentEnrollments
          .filter(e => typeof e.customId === 'string' && e.customId.startsWith('enr-'))
          .map(e => parseInt(e.customId.replace('enr-', ''), 10))
        
        if (validIds.length > 0) {
          nextIdNumber = Math.max(...validIds) + 1
        }
      }
      
      const newCustomId = 'enr-' + String(nextIdNumber).padStart(3, '0')

      // Összerakjuk a payload-ot: az 'id'-t KIHAGYJUK, hogy a json-server azt tegyen vele amit akar,
      // mi viszont beküldjük a saját, tiszta 'customId' mezőnket!
      const payload = {
        customId: newCustomId,
        courseId: studentData.courseId,
        studentName: studentData.studentName,
        email: studentData.email,
        phone: studentData.phone,
        enrolledAt: new Date().toISOString()
      }

      const xhrPost = new XMLHttpRequest()
      xhrPost.open('POST', 'http://localhost:3001/enrollments', true)
      xhrPost.setRequestHeader('Content-Type', 'application/json;charset=UTF-8')
      
      xhrPost.onreadystatechange = function () {
        if (xhrPost.readyState === 4 && xhrPost.status === 201) {
          Swal.fire({
            icon: 'success',
            title: 'Sikeres jelentkezés!',
            text: `A regisztrációd rögzítve lett ${newCustomId} egyedi azonosítóval.`,
            confirmButtonColor: '#FFCC00',
            background: 'var(--bg-main)',
            color: 'var(--text-main)'
          })
          if (studentFormRef.value) studentFormRef.value.resetForm()
        }
      }
      xhrPost.send(JSON.stringify(payload))
    }
  }
  xhrGet.send()
}

// TANÁR JELENTKEZÉS BEKÜLDÉSE (XHR POST -> /applications)
const handleApplicationSubmit = (teacherData) => {
  const xhrGet = new XMLHttpRequest()
  xhrGet.open('GET', 'http://localhost:3001/applications', true)
  
  xhrGet.onreadystatechange = function () {
    if (xhrGet.readyState === 4 && xhrGet.status === 200) {
      const currentApplications = JSON.parse(xhrGet.responseText)
      
      let nextIdNumber = 1
      if (currentApplications.length > 0) {
        const validIds = currentApplications
          .filter(a => typeof a.customId === 'string' && a.customId.startsWith('app-'))
          .map(a => parseInt(a.customId.replace('app-', ''), 10))
          
        if (validIds.length > 0) {
          nextIdNumber = Math.max(...validIds) + 1
        }
      }
      
      const newCustomId = 'app-' + String(nextIdNumber).padStart(3, '0')

      const payload = {
        customId: newCustomId,
        fullName: teacherData.fullName,
        email: teacherData.email,
        phone: teacherData.phone,
        role: teacherData.role,
        targetCategory: teacherData.targetCategory,
        experienceYears: teacherData.experienceYears,
        linkedinProfile: teacherData.linkedinProfile || null,
        introduction: teacherData.introduction,
        status: 'Feldolgozás alatt',
        appliedAt: new Date().toISOString()
      }

      const xhrPost = new XMLHttpRequest()
      xhrPost.open('POST', 'http://localhost:3001/applications', true)
      xhrPost.setRequestHeader('Content-Type', 'application/json;charset=UTF-8')
      
      xhrPost.onreadystatechange = function () {
        if (xhrPost.readyState === 4 && xhrPost.status === 201) {
          Swal.fire({
            icon: 'success',
            title: 'Sikeres mentori regisztráció!',
            text: `Jelentkezésed elmentve ${newCustomId} egyedi azonosítóval.`,
            confirmButtonColor: '#FFCC00',
            background: 'var(--bg-main)',
            color: 'var(--text-main)'
          })
          if (teacherFormRef.value) teacherFormRef.value.resetForm()
        }
      }
      xhrPost.send(JSON.stringify(payload))
    }
  }
  xhrGet.send()
}
onMounted(() => {
  loadFormData()
})
</script>

<template>
  <section id="join-us" class="py-5 bg-section-join">
    <div class="container px-4">
      
      <div class="row justify-content-center mb-5">
        <div class="col-md-6 text-center">
          <div class="btn-group w-100 shadow-sm" role="group" aria-label="Jelentkezési kategóriák">
            <button 
              type="button" 
              class="btn py-2 fw-bold transition-btn"
              :class="activeForm === 'student' ? 'btn-active' : 'btn-inactive'"
              @click="activeForm = 'student'"
            >
              Kurzusra jelentkezem (Diák)
            </button>
            <button 
              type="button" 
              class="btn py-2 fw-bold transition-btn"
              :class="activeForm === 'teacher' ? 'btn-active' : 'btn-inactive'"
              @click="activeForm = 'teacher'"
            >
              Csapathoz csatlakozom (Tanár)
            </button>
          </div>
        </div>
      </div>

      <div 
        class="row align-items-center g-5 transition-row" 
        :class="{ 'flex-row-reverse': activeForm === 'teacher' }"
      >
        <div class="col-lg-6 text-start" id="about">
          <div v-if="activeForm === 'student'">
            <h2 class="display-6 fw-bold mb-3">Indítsd el a karriered még ma!</h2>
            <p class="text-muted lead">
              Csatlakozz gyakorlatorientált képzéseinkhez. Tapasztalt mentorok segítségével, 
              élő projekteken keresztül sajátíthatod el a piac legkeresettebb technológiáit.
            </p>
            <ul class="list-unstyled mt-4 text-muted">
              <li class="mb-2"><span id="mark">✓</span> Élő mentorálás és heti konzultációk</li>
              <li class="mb-2"><span id="mark">✓</span> Örökös hozzáférés a tananyagokhoz</li>
              <li class="mb-2"><span id="mark">✓</span> Sanhilzon igazolás a sikeres elvégzésről</li>
            </ul>
          </div>
          <div v-else>
            <h2 class="display-6 fw-bold mb-3">Add át a tudásod, legyél mentor!</h2>
            <p class="text-muted lead">
              A Sanhilzon csapatánál hiszünk a közösség erejében. Ha van legalább néhány év 
              szakmai tapasztalatod, és szívesen segítenéd a jövő generációját, nálunk a helyed.
            </p>
            <ul class="list-unstyled mt-4 text-muted">
              <li class="mb-2"><span id="mark">✓</span> Flexibilis időbeosztás, távmunka</li>
              <li class="mb-2"><span id="mark">✓</span> Versenyképes juttatások</li>
              <li class="mb-2"><span id="mark">✓</span> Egy összetartó, modern fejlesztői közösség</li>
            </ul>
          </div>
        </div>

        <div class="col-lg-6">
          <div class="card p-4 rounded-4 shadow-sm border-0 bg-card-custom">
            <CourseForm 
              v-if="activeForm === 'student'" 
              ref="studentFormRef"
              :courses="courses"
              @submit-enrollment="handleEnrollmentSubmit"
            />
            <TeacherForm 
              v-else 
              ref="teacherFormRef"
              :categories="categories"
              @submit-application="handleApplicationSubmit"
            />
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<style scoped>
    .bg-section-join {
    background-color: var(--bg-card); /* Enyhén eltérő háttér az elválasztásért */
    padding-top: 5rem;
    padding-bottom: 5rem;
    }

    /* Egyedi stílusok a választó gombcsoporthoz */
    .transition-btn {
    transition: all var(--transition-speed) ease;
    border: 1px solid var(--border-color) !important;
    }

    .btn-active {
    background-color: var(--sh-yellow) !important;
    color: var(--sh-dark) !important;
    }

    .btn-inactive {
    background-color: var(--bg-main) !important;
    color: var(--text-muted) !important;
    }

    /* Finom animációs hatás a tükröződés váltásakor */
    .transition-row {
    transition: all 0.5s ease-in-out;
    }

    .bg-card-custom {
    background-color: var(--bg-main) !important;
    }

    #about h2 {
        color: var(--text-main) !important;
    }

    #about p, #about li {
        color: var(--text-muted) !important;
    }

    #about li:hover {
        color: var(--sh-yellow) !important;
        transition: all var(--transition-speed) ease;
    }

    #mark {
        color: var(--sh-yellow);
        font-weight: bold;
        margin-right: 0.5rem;
    }
</style>