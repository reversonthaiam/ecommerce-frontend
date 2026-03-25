<template>
  <nav
    class="sticky top-0 z-50 bg-brand-dark/80 backdrop-blur-xl border-b border-brand-purple-dark/30 px-8 py-5 flex justify-between items-center transition-all duration-300">

    <div class="flex items-center gap-12">
      <router-link to="/dashboard" class="group flex items-center gap-2">
        <div
          class="w-8 h-8 bg-brand-pink rounded-lg shadow-[0_0_15px_rgba(250,166,255,0.4)] group-hover:rotate-12 transition-transform duration-300">
        </div>
        <span class="text-2xl font-black text-brand-pink-pale tracking-tighter">
          NEXUS<span class="text-brand-pink">.</span>
        </span>
      </router-link>

      <div class="hidden md:flex gap-8">
        <router-link v-for="link in navLinks" :key="link.path" :to="link.path"
          class="relative text-sm font-bold uppercase tracking-widest text-brand-pink-pale/50 hover:text-brand-pink transition-colors duration-300 py-1 group"
          active-class="text-brand-pink">
          {{ link.label }}
          <span class="absolute bottom-0 left-0 w-0 h-0.5 bg-brand-pink transition-all duration-300 group-hover:w-full"
            :class="{ 'w-full': $route.path === link.path }">
          </span>
        </router-link>
      </div>
    </div>

    <div class="flex items-center gap-6">
      <button @click="handleLogout"
        class="relative flex items-center gap-2 bg-brand-purple-dark/20 border border-brand-purple-dark/50 text-brand-pink-pale px-5 py-2 rounded-xl font-bold text-sm transition-all hover:bg-brand-pink hover:text-brand-dark hover:border-brand-pink active:scale-95 group">
        <span>Logout</span>
        <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 group-hover:translate-x-1 transition-transform"
          fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3"
            d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
        </svg>
      </button>
    </div>
  </nav>
</template>

<script setup lang="ts">
import { useRouter, useRoute } from 'vue-router'
import { useAuthStore } from '../stores/auth'

const router = useRouter()
const route = useRoute()
const authStore = useAuthStore()

const navLinks = [
  { label: 'Dashboard', path: '/dashboard' },
  { label: 'Orders', path: '/orders' },
  { label: 'Products', path: '/products' },
  { label: 'New Product', path: '/newProducts' },
]

function handleLogout() {
  authStore.logout()
  router.push('/login')
}
</script>

<style scoped>
/* Estilo para o link ativo do Vue Router */
.router-link-active {
  color: var(--color-brand-pink) !important;
}
</style>
