<script setup>
import { ref } from 'vue';

const passwordLength = ref(10);
const generatedPassword = ref('');
const isCopied = ref(false);
const attemptedCopy = ref(false);

const generatePassword = () => {
  const charset = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_=+';
  let password = '';
  for (let i = 0; i < passwordLength.value; ++i) {
    const randomIndex = Math.floor(Math.random() * charset.length);
    password += charset[randomIndex];
  }
  generatedPassword.value = password;
  isCopied.value = false;
  attemptedCopy.value = false;
}

const copyPassword = () => {
  if (!generatedPassword.value) {
    attemptedCopy.value = true;
    return;
  }
  const inputField = document.getElementById('password');
  inputField.select();
  document.execCommand('copy', false, null);
  isCopied.value = true;
}
</script>
<template>
  <div class="bg-gradient-to-b from-gray-900 to-blue-900 min-h-screen flex items-center justify-center">
    <div class="mx-auto py-24 max-w-3xl text-center">
      <div class="container mx-auto my-8 bg-gradient-to-br from-gray-800 to-gray-900 rounded-lg p-8">
        <h1 class="text-3xl font-semibold mb-4 text-white">Random Password Generator</h1>
        <div class="flex flex-col space-y-4">
          <div>
            <label for="length" class="block mb-2 text-white">Password Length:</label>
            <input type="number" id="length" v-model.number="passwordLength" class="border px-2 py-2 rounded-md border-slate-400">
          </div>
          <div class="flex flex-col">
            <label for="password" class="block mb-2 text-white">Generated Password:</label>
            <div class="flex items-center justify-center space-x-2">
              <input type="text" id="password" :value="generatedPassword" class="border px-2 py-2 rounded-md border-slate-400" readonly>
              <button @click="copyPassword" class="bg-gray-300 text-gray-700 px-4 py-2 rounded hover:bg-gray-400">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2m-6 0V3a2 2 0 012-2h2a2 2 0 012 2v2m-4 0h8"></path>
                </svg>
              </button>
            </div>
            <p v-if="!generatedPassword && attemptedCopy" class="text-red-500">Please generate a password first.</p>
            <p v-else-if="isCopied" class="text-green-500">Password copied!</p>
          </div>
          <div>
            <button @click="generatePassword" class="bg-blue-500 text-white px-4 py-2 rounded">Generate Password</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
