<template>
  <div class="min-h-screen bg-brand-dark flex items-center justify-center p-6 select-none">

    <div class="relative w-full max-w-[400px]">
      <project-modal />
      <div class="absolute -top-10 -left-10 w-32 h-32 bg-brand-purple rounded-full blur-[80px] opacity-20"></div>
      <div class="absolute -bottom-10 -right-10 w-32 h-32 bg-brand-pink rounded-full blur-[80px] opacity-20"></div>

      <div
        class="relative bg-brand-purple-dark/10 backdrop-blur-xl border border-brand-purple-dark/30 p-10 rounded-[2rem] shadow-2xl">

        <header class="text-center mb-10">
          <h1 class="text-4xl font-black text-brand-pink-pale tracking-tighter mb-2">
            Welcome <span class="text-brand-pink">Back.</span>
          </h1>
          <p class="text-brand-pink-pale/50 text-sm font-medium">Enter your credentials to access the panel</p>
        </header>

        <form @submit.prevent="handleLogin" class="flex flex-col gap-5">
          <div class="flex flex-col gap-2">
            <label class="text-xs font-bold uppercase tracking-widest text-brand-pink/60 ml-1">Email Address</label>
            <input v-model="email" type="email" placeholder="name@company.com"
              class="bg-brand-dark/40 border border-brand-purple-dark/50 text-white p-4 rounded-2xl outline-none focus:border-brand-pink focus:ring-1 focus:ring-brand-pink/30 transition-all placeholder:text-white/20"
              required />
          </div>

          <div class="flex flex-col gap-2">
            <label class="text-xs font-bold uppercase tracking-widest text-brand-pink/60 ml-1">Password</label>
            <input v-model="password" type="password" placeholder="••••••••"
              class="bg-brand-dark/40 border border-brand-purple-dark/50 text-white p-4 rounded-2xl outline-none focus:border-brand-pink focus:ring-1 focus:ring-brand-pink/30 transition-all placeholder:text-white/20"
              required />
          </div>

          <button type="submit"
            class="group relative bg-brand-pink text-brand-dark font-black p-4 rounded-2xl mt-4 overflow-hidden transition-all hover:scale-[1.02] active:scale-[0.98] shadow-lg shadow-brand-pink/20">
            <div
              class="absolute inset-0 bg-white/20 translate-y-full group-hover:translate-y-0 transition-transform duration-300">
            </div>
            <span class="relative">Sign in to Account</span>
          </button>
        </form>

        <footer class="mt-8 text-center">
          <p class="text-brand-pink-pale/40 text-sm">
            Don't have an account?
            <router-link to="/register"
              class="text-brand-pink hover:text-brand-pink-pale font-bold transition-colors underline underline-offset-4">
              Create one
            </router-link>
          </p>

          <Transition name="fade">
            <p v-if="error"
              class="bg-red-500/10 border border-red-500/20 text-red-400 text-xs py-2 px-4 rounded-lg mt-4 font-medium italic">
              ⚠ {{ error }}
            </p>
          </Transition>
        </footer>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '../stores/auth'
import ProjectModal from '@/components/ProjectModal.vue'

const router = useRouter()
const authStore = useAuthStore()

const email = ref('')
const password = ref('')
const error = ref('')

async function handleLogin() {
  error.value = ''
  try {
    await authStore.login(email.value, password.value)
    router.push('/products')
  } catch (e) {
    error.value = 'Invalid email or password. Please try again.'
  }
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
