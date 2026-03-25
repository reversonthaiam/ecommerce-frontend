<template>
  <div class="min-h-screen bg-brand-dark p-8">
    <header class="mb-10">
      <h1 class="text-4xl font-black text-brand-pink-pale tracking-tighter">
        System <span class="text-brand-pink">Overview</span>
      </h1>
      <p class="text-brand-pink-pale/40 font-medium">Real-time performance metrics</p>
    </header>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

      <div
        class="relative overflow-hidden group bg-brand-purple-dark/10 backdrop-blur-md border border-brand-purple-dark/30 rounded-[2.5rem] p-8 transition-all hover:border-brand-pink/40 shadow-2xl">
        <div
          class="absolute -top-10 -right-10 w-32 h-32 bg-brand-pink rounded-full blur-[60px] opacity-10 group-hover:opacity-20 transition-opacity">
        </div>

        <h2 class="text-xs font-black uppercase tracking-[0.2em] text-brand-pink/60 mb-4">Total sales today</h2>
        <div class="flex items-baseline gap-2">
          <span class="text-brand-pink-pale/50 text-xl font-bold">R$</span>
          <p class="text-5xl font-black text-white tracking-tighter">
            {{ dashboardData.total_sales_today?.toLocaleString('pt-BR', { minimumFractionDigits: 2 }) || '0,00' }}
          </p>
        </div>
        <div class="mt-6 flex items-center gap-2 text-green-400 text-sm font-bold">
          <span class="flex h-2 w-2 rounded-full bg-green-400 animate-pulse"></span>
          Live updating
        </div>
      </div>

      <div
        class="bg-brand-purple-dark/5 backdrop-blur-md border border-brand-purple-dark/20 rounded-[2.5rem] p-8 transition-all hover:bg-brand-purple-dark/10">
        <h2 class="text-xs font-black uppercase tracking-[0.2em] text-brand-pink-pale/40 mb-6">Best selling products
        </h2>
        <div class="space-y-4">
          <div v-for="(quantity, name) in dashboardData.best_selling_products" :key="name"
            class="flex items-center justify-between p-4 rounded-2xl bg-brand-dark/40 border border-brand-purple-dark/30 group hover:border-brand-pink/30 transition-colors">
            <span class="text-brand-pink-pale font-bold">{{ name }}</span>
            <span class="bg-brand-pink/10 text-brand-pink px-3 py-1 rounded-lg text-xs font-black">
              {{ quantity }} units
            </span>
          </div>
          <div v-if="!dashboardData.best_selling_products" class="text-brand-pink-pale/20 italic text-sm">No data
            available</div>
        </div>
      </div>

      <div
        class="bg-brand-purple-dark/5 backdrop-blur-md border border-brand-purple-dark/20 rounded-[2.5rem] p-8 transition-all hover:bg-brand-purple-dark/10">
        <h2 class="text-xs font-black uppercase tracking-[0.2em] text-brand-pink-pale/40 mb-6">Revenue by category</h2>
        <div class="space-y-4">
          <div v-for="(revenue, name) in dashboardData.revenue_by_category" :key="name" class="relative">
            <div class="flex justify-between text-sm mb-2">
              <span class="text-brand-pink-pale/80 font-medium">{{ name }}</span>
              <span class="text-brand-pink font-bold">R$ {{ revenue }}</span>
            </div>
            <div class="h-1.5 w-full bg-brand-dark rounded-full overflow-hidden">
              <div class="h-full bg-brand-purple rounded-full" style="width: 65%"></div>
            </div>
          </div>
          <div v-if="!dashboardData.revenue_by_category" class="text-brand-pink-pale/20 italic text-sm">No data
            available</div>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import api from '@/services/api'

const dashboardData = ref<any>({})

onMounted(async () => {
  try {
    const response = await api.get('/dashboard')
    dashboardData.value = response.data
  } catch (e) {
    console.error("Failed to fetch dashboard data")
  }
})
</script>
