<template>
  <div class="min-h-screen bg-brand-dark p-8 pb-20">
    <header class="mb-12">
      <h1 class="text-4xl font-black text-brand-pink-pale tracking-tighter">
        My <span class="text-brand-pink">Orders</span>
      </h1>
      <p class="text-brand-pink-pale/40 font-medium">Track your purchase history</p>
    </header>

    <div v-if="orders.length === 0" class="flex flex-col items-center justify-center py-24 gap-4">
      <span class="text-6xl">📦</span>
      <p class="text-brand-pink-pale/40 font-bold text-lg">No orders yet</p>
    </div>

    <div class="flex flex-col gap-4">
      <div v-for="order in orders" :key="order.id"
        class="group bg-brand-purple-dark/5 backdrop-blur-md border border-brand-purple-dark/20 rounded-[2rem] p-6 transition-all duration-300 hover:bg-brand-purple-dark/20 hover:border-brand-pink/20">

        <div class="flex justify-between items-center">
          <div>
            <p class="text-brand-pink font-black text-lg">Order #{{ order.id }}</p>
            <p class="text-brand-pink-pale/40 text-sm font-medium mt-1">{{ order.created_at }}</p>
          </div>

          <div class="flex items-center gap-4">
            <span
              :class="order.status === 'concluido' ? 'bg-green-500/10 text-green-400 border-green-500/30' : 'bg-yellow-500/10 text-yellow-400 border-yellow-500/30'"
              class="px-4 py-1 rounded-xl text-xs font-black uppercase tracking-widest border">
              {{ order.status }}
            </span>
            <p class="text-brand-pink font-black text-xl">${{ order.total }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import api from '@/services/api'
import type { Order } from '@/types/Order'
import { onMounted, ref } from 'vue'

const orders = ref<Order[]>([])

onMounted(async () => {
  try {
    const response = await api.get('/orders')
    orders.value = response.data
  } catch (e) {
    console.error('Failed to load orders')
  }
})
</script>
