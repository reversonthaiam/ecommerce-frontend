<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <div class="bg-white p-8 rounded-lg shadow-md w-96">
      <h1 class="text-2xl font-bold text-center mb-6">Criar conta</h1>
      <form @submit.prevent="handleRegister" class="flex flex-col gap-4">
        <input v-model="email" type="email" placeholder="Email"
          class="border rounded p-2 outline-none focus:border-blue-500" />
        <input v-model="password" type="password" placeholder="Senha"
          class="border rounded p-2 outline-none focus:border-blue-500" />
        <input v-model="passwordConfirmation" type="password" placeholder="Confirmar senha"
          class="border rounded p-2 outline-none focus:border-blue-500" />
        <button type="submit" class="bg-blue-500 text-white p-2 rounded hover:bg-blue-600">
          Criar conta
        </button>
      </form>
      <p class="text-center mt-4 text-sm">
        Já tem conta?
        <router-link to="/login" class="text-blue-500 hover:underline">Entrar</router-link>
      </p>
      <p v-if="error" class="text-red-500 text-center mt-2">{{ error }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '../stores/auth'

const router = useRouter()
const authStore = useAuthStore()

const email = ref('')
const password = ref('')
const passwordConfirmation = ref('')
const error = ref('')

async function handleRegister() {
  try {
    await authStore.register(email.value, password.value, passwordConfirmation.value)
    router.push('/login')
  } catch (e) {
    error.value = 'Erro ao criar conta'
  }
}
</script>
