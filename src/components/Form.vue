<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <h1 class="text-center mb-3">User Information Form / Credentials</h1>

        <form @submit.prevent="submitForm">
          <!-- Username -->
          <div class="mb-3">
            <label class="form-label" for="username">Username</label>
            <input
              id="username"
              type="text"
              class="form-control"
              v-model="formData.username"
              :class="{ 'is-invalid': errors.username }"
            />
            <div class="invalid-feedback">{{ errors.username }}</div>
          </div>

          <!-- Password -->
          <div class="mb-3">
            <label class="form-label" for="password">Password</label>
            <input id="password" type="password" class="form-control" v-model="formData.password" />
          </div>

          <!-- Australian Resident -->
          <div class="form-check mb-3">
            <input
              id="isAustralian"
              type="checkbox"
              class="form-check-input"
              v-model="formData.isAustralian"
            />
            <label class="form-check-label" for="isAustralian">Australian Resident?</label>
          </div>

          <!-- Reason -->
          <div class="mb-3">
            <label class="form-label" for="reason">Reason For Joining</label>
            <textarea
              id="reason"
              class="form-control"
              rows="3"
              v-model="formData.reason"
            ></textarea>
          </div>

          <!-- Gender -->
          <div class="mb-4">
            <label class="form-label" for="gender">Gender</label>
            <select id="gender" class="form-select" v-model="formData.gender">
              <option value="" disabled selected>Select…</option>
              <option value="female">Female</option>
              <option value="male">Male</option>
              <option value="other">Other</option>
            </select>
          </div>

          <!-- Buttons -->
          <button type="submit" class="btn btn-primary me-2">Submit</button>
          <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
        </form>
        <div class="row mt-5" v-if="submittedCards.length">
          <div class="row">
            <div
              v-for="(card, index) in submittedCards"
              :key="index"
              class="col-12 col-sm-6 col-md-4 col-lg-3 mb-3"
            >
              <div class="card" style="width: 100%">
                <div class="card-header">User Information</div>
                <ul class="list-group list-group-flush">
                  <li class="list-group-item">Username: {{ card.username }}</li>
                  <li class="list-group-item">Password: {{ card.password }}</li>
                  <li class="list-group-item">
                    Australian Resident: {{ card.isAustralian ? 'Yes' : 'No' }}
                  </li>
                  <li class="list-group-item">Gender: {{ card.gender }}</li>
                  <li class="list-group-item">Reason: {{ card.reason }}</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const errors = ref({
  username: '',
  password: '',
  gender: '',
})

const formData = ref({
  username: '',
  password: '',
  isAustralian: false,
  reason: '',
  gender: '',
})

const submittedCards = ref([])

const submitForm = () => {
  errors.value = {
    username: '',
    password: '',
    gender: '',
  }

  let isValid = true

  if (!formData.value.username.trim()) {
    errors.value.username = 'Username is required.'
    isValid = false
  }

  if (!formData.value.password.trim()) {
    errors.value.password = 'Password is required.'
    isValid = false
  }

  if (!formData.value.gender) {
    errors.value.gender = 'Please select a gender.'
    isValid = false
  }

  if (isValid) {
    submittedCards.value.push({ ...formData.value })
    clearForm()
  }
}
</script>
<style scoped>
.card {
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card-header {
  background-color: #275fda;
  color: white;
  padding: 10px;
  border-radius: 10px 10px 0 0;
}

.list-group-item {
  padding: 10px;
}
</style>
