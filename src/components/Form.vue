<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">User Information Form</h1>

    <form @submit.prevent="submitForm">
      <div class="row mb-3">
        <div class="col">
          <label for="username" class="form-label">Username</label>
          <input
            type="text"
            class="form-control"
            id="username"
            v-model="formData.username"
            required
          />
          <div class="form-text">3–15 characters, letters/numbers/underscore only.</div>
          <div class="invalid-feedback">Please enter a valid username.</div>
        </div>

        <div class="col">
          <label for="password" class="form-label">Password</label>
          <input
            type="password"
            class="form-control"
            id="password"
            minlength="4"
            maxlength="10"
            v-model="formData.password"
          />
          <div class="form-text">4–10 characters.</div>
          <div class="invalid-feedback">Please enter a password (min 6 characters).</div>
        </div>
      </div>

      <div class="form-check mb-3">
        <input
          type="checkbox"
          class="form-check-input"
          id="isAustralian"
          v-model="formData.isAustralian"
        />
        <label class="form-check-label" for="isAustralian">Australian Resident?</label>
      </div>

      <div class="mb-3">
        <label for="gender" class="form-label">Gender</label>
        <select class="form-select" id="gender" v-model="formData.gender" required>
          <option value="" disabled>Select your gender</option>
          <option value="female">Female</option>
          <option value="male">Male</option>
          <option value="other">Other</option>
        </select>
        <div class="invalid-feedback">Please select a gender.</div>
      </div>

      <div class="mb-3">
        <label for="reason" class="form-label">Reason for Joining</label>
        <textarea
          class="form-control"
          id="reason"
          rows="3"
          v-model="formData.reason"
          maxlength="200"
        ></textarea>
        <div class="form-text">Optional, max 200 characters.</div>
      </div>

      <button type="submit" class="btn btn-primary me-2">Submit</button>
      <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
    </form>

    <div class="row mt-5" v-if="submittedCards.length">
      <div class="d-flex flex-wrap justify-content-start">
        <div
          v-for="(card, index) in submittedCards"
          :key="index"
          class="card m-2"
          style="width: 18rem"
        >
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
</template>

<script setup>
import { ref, computed } from 'vue'

const formData = ref({
  username: '',
  password: '',
  isAustralian: false,
  gender: '',
  reason: '',
})

const submittedCards = ref([])
const triedSubmit = ref(false)

const usernameOK = computed(() => /^[A-Za-z0-9_]{3,15}$/.test(formData.value.username))
const passwordOK = computed(() => formData.value.password.trim().length >= 6)
const genderOK = computed(() => !!formData.value.gender)

const submitForm = (e) => {
  const formEl = e.target
  if (!formEl.checkValidity()) {
    formEl.reportValidity()
    return
  }
  submittedCards.value.push({ ...formData.value })
  clearForm()
}

const clearForm = () => {
  formData.value = {
    username: '',
    password: '',
    isAustralian: false,
    gender: '',
    reason: '',
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
  color: #fff;
  padding: 10px;
  border-radius: 10px 10px 0 0;
}
.list-group-item {
  padding: 10px;
}
</style>
