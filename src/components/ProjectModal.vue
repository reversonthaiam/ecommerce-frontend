<template>
  <Transition name="modal">
    <div v-if="isOpen" class="fixed inset-0 z-50 flex items-center justify-center p-4">
      <div class="absolute inset-0 bg-brand-dark/80 backdrop-blur-md" @click="close" />

      <div
        class="relative bg-brand-dark border border-brand-purple-dark/50 rounded-[2.5rem] p-10 max-w-lg w-full shadow-2xl shadow-brand-pink/10">

        <div class="flex items-center gap-3 mb-6">
          <div class="w-8 h-8 bg-brand-pink rounded-lg shadow-[0_0_15px_rgba(250,166,255,0.4)]"></div>
          <h2 class="text-2xl font-black text-brand-pink-pale tracking-tighter">
            NEXUS<span class="text-brand-pink">.</span>
          </h2>
        </div>

        <p class="text-brand-pink-pale/70 font-medium leading-relaxed mb-6">
          A full-stack e-commerce platform built as a portfolio project, demonstrating modern backend and frontend
          architecture.
        </p>

        <div class="mb-6 p-4 rounded-2xl bg-brand-purple-dark/20 border border-brand-pink/20">
          <p class="text-xs font-black uppercase tracking-widest text-brand-pink/60 mb-3">Admin Access</p>
          <div class="flex flex-col gap-2">
            <div class="flex justify-between items-center">
              <span class="text-brand-pink-pale/50 text-sm font-medium">Email</span>
              <span class="text-brand-pink font-black text-sm">admin@admin.com</span>
            </div>
            <div class="flex justify-between items-center">
              <span class="text-brand-pink-pale/50 text-sm font-medium">Password</span>
              <span class="text-brand-pink font-black text-sm">123456</span>
            </div>
            <p class="text-brand-pink-pale/30 text-xs mt-1">→ Access dashboard and manage products</p>
          </div>
        </div>

        <div class="mb-6">
          <p class="text-xs font-black uppercase tracking-widest text-brand-pink/60 mb-4">Tech Stack</p>
          <div class="flex flex-wrap gap-2">
            <span v-for="tech in techs" :key="tech"
              class="bg-brand-purple-dark/30 border border-brand-purple-dark/50 text-brand-pink-pale px-3 py-1 rounded-xl text-xs font-bold">
              {{ tech }}
            </span>
          </div>
        </div>

        <div class="mb-8">
          <p class="text-xs font-black uppercase tracking-widest text-brand-pink/60 mb-4">Key Features</p>
          <ul class="flex flex-col gap-2">
            <li v-for="feature in features" :key="feature"
              class="flex items-center gap-2 text-brand-pink-pale/60 text-sm">
              <span class="text-brand-pink">→</span> {{ feature }}
            </li>
          </ul>
        </div>

        <button @click="close"
          class="w-full bg-brand-pink text-brand-dark font-black py-4 rounded-2xl hover:scale-[1.02] active:scale-[0.98] transition-all shadow-xl shadow-brand-pink/20">
          Explore the Store
        </button>
      </div>
    </div>
  </Transition>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const isOpen = ref(true)

const techs = [
  'Ruby on Rails 8', 'PostgreSQL', 'Redis', 'Sidekiq',
  'Devise + JWT', 'Active Storage', 'Vue 3', 'TypeScript',
  'Tailwind CSS', 'Pinia', 'Docker'
]

const features = [
  'JWT Authentication with role-based access',
  'Async email confirmation via Sidekiq',
  'Redis cache for dashboard queries',
  'Image upload with Active Storage',
  'Admin dashboard with sales metrics',
]

function close() {
  isOpen.value = false
}
</script>

<style scoped>
.modal-enter-active,
.modal-leave-active {
  transition: all 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
</style>
