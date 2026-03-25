<template>
  <nav class="bg-blue-600 text-white p-4 flex justify-between items-center">
    <div class="flex gap-4">
      <router-link to="/dashboard" class="hover:underline">Dashboard</router-link>
      <router-link to="/orders" class="hover:underline">Orders</router-link>
      <router-link to="/products" class="hover:underline">Products</router-link>
      <router-link to="/newProducts" class="hover:underline">New Products</router-link>
    </div>
    <button @click="handleLogout" class="bg-white text-blue-600 px-4 py-1 rounded hover:bg-gray-100">
      Logout
    </button>
  </nav>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
import { useAuthStore } from '../stores/auth'

const router = useRouter()
const authStore = useAuthStore()

function handleLogout() {
  authStore.logout()
  router.push('/login')
}
</script>
