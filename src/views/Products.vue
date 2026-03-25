<template>
  <div class="p-8">
    <h1 class="text-2xl font-bold mb-6">Choose your product</h1>

    <p v-if="message" class="mb-4 text-green-500">{{ message }}</p>

    <section class="grid grid-cols-3 gap-4">
      <div v-for="product in products" :key="product.id" class="bg-white rounded-lg shadow p-4">
        <img v-if="product.image_url" :src="product.image_url" :alt="product.name"
          class="w-full h-48 object-cover rounded mb-3" />
        <div v-else class="w-full h-48 bg-gray-200 rounded mb-3 flex items-center justify-center">
          <p class="text-gray-400">No image</p>
        </div>
        <p class="font-bold">{{ product.name }}</p>
        <p class="text-gray-500">R$ {{ product.price }}</p>
        <button @click="handleBuy(product.id)"
          class="mt-4 w-full bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
          Buy
        </button>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import api from '@/services/api'

const products = ref([])
const message = ref('')

onMounted(async () => {
  const response = await api.get('/products')
  products.value = response.data
})

const handleBuy = async (id: number) => {
  try {
    await api.post('/orders', {
      order: {
        items: [
          { product_id: id, quantity: 1 }
        ]
      }
    })
    message.value = 'Order placed successfully!'
  } catch (e) {
    message.value = 'Error placing order'
  }
}
</script>
