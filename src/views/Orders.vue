<template>
  <div class="p-8">
    <h1 class="text-2xl font-bold mb-6">Meus Pedidos</h1>

    <div class="flex flex-col gap-4">
      <div v-for="order in orders" :key="order.id" class="bg-white rounded-lg shadow p-4">
        <p class="font-bold">Pedido #{{ order.id }}</p>
        <p class="text-gray-500">Status: {{ order.status }}</p>
        <p class="text-gray-500">Total: R$ {{ order.total }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import api from '@/services/api'
import { onMounted, ref } from 'vue'

const orders = ref([])

onMounted(async () => {
  const response = await api.get('/orders')
  orders.value = response.data
})
</script>
