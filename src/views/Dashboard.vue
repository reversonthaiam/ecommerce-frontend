<template>
  <div class="p-8">
    <h1 class="text-2xl font-bold mb-6">Dashboard</h1>

    <div class="grid grid-cols-3 gap-4">
      <div class="bg-white rounded-lg shadow p-4">
        <h2 class="font-bold text-gray-500 mb-2">Total de vendas hoje</h2>
        <p class="text-2xl font-bold">R$ {{ dashboardData.total_sales_today }}</p>
      </div>

      <div class="bg-white rounded-lg shadow p-4">
        <h2 class="font-bold text-gray-500 mb-2">Produtos mais vendidos</h2>
        <div v-for="(quantity, name) in dashboardData.best_selling_products" :key="name">
          <p>{{ name }}: {{ quantity }} unidades</p>
        </div>
      </div>

      <div class="bg-white rounded-lg shadow p-4">
        <h2 class="font-bold text-gray-500 mb-2">Receita por categoria</h2>
        <div v-for="(revenue, name) in dashboardData.revenue_by_category" :key="name">
          <p>{{ name }}: R$ {{ revenue }}</p>
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
  const response = await api.get('/dashboard')
  dashboardData.value = response.data
})
</script>
