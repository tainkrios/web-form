<template>
  <form @submit.prevent="handleSubmit">
    <label>Email:</label>
    <input 
      type="email" 
      required
      v-model="state.email"
    >
    <label>Password:</label>
    <input 
      type="password" 
      required
      v-model="state.password"
    >
    <div 
      class="error"
      v-if="state.passwordErr"
    >{{ state.passwordErr }}</div>
    <label>Role:</label>
    <select v-model="state.role">
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>
    <label>Skills:</label>
    <input 
      type="text" 
      v-model="state.tempSkills"
      @keyup="addSkill"
    >
    <div 
      v-for="(skill, i) in state.skills"
      :key="skill"
      class="pill"
      @click="deleteSkill(i)"
    >
      {{ skill }}
    </div>
    <div class="terms">
      <input 
        type="checkbox"
        v-model="state.terms"
      >
      <label>Accept terms and conditions</label>
    </div>

    <div class="submit">
      <button type="submit">Create an Account</button>
    </div>
  </form>
</template>

<script setup lang="ts">
import { reactive } from 'vue'

interface State {
  email: string
  password: string
  role: string
  terms: boolean
  tempSkills: string
  skills: []
  passwordErr: string
}

const state = reactive<State>(
  {
    email: '',
    password: '',
    role: '',
    terms: false,
    tempSkills: '',
    skills: [],
    passwordErr: ''
  }
)

function addSkill(e: { key: string; }) {
  if (e.key === 'Enter' && state.tempSkills) {
    if (!state.skills.includes(state.tempSkills)) {
      state.skills.push(state.tempSkills)
    }
    state.tempSkills = ''
    
  }
}

function deleteSkill(i: number) {
  console.log(i);
  state.skills.splice(i, 1);
}
function handleSubmit() {
  //validate pass
  state.passwordErr = state.password.length > 5 ? 
    '' : 'Password must be at least 6 caracters long'
}
</script>

<style scoped>
form {
  max-width: 420px;
  margin: 30px auto;
  background: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}
label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
input, select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
input[type='checkbox'] {
  display: inline-block;
  width: 16px;
  margin: 0 16px 0 0;
  position: relative;
  top: 2px;
}
.pill {
  display: inline-block;
  margin: 20px 10px 0 0;
  padding: 6px 12px;
  background: #eee;
  border-radius: 20px;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #777;
  cursor: pointer;
}
button {
  background: #0b6dff;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  color: white;
  border-radius: 20px;
}
.submit {
  text-align: center;
}
.error {
  color: #ff0062;
  margin-top: 10px;
  font-size: 0.8em;
  font-weight: bold;
}
</style>