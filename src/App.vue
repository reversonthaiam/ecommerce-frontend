<template>
  <Navbar v-if="authStore.isAuthenticated" />
  <router-view />
</template>

<script setup lang="ts">
import Navbar from './components/Navbar.vue'
import { useAuthStore } from './stores/auth'

const authStore = useAuthStore()
</script>
