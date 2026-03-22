<template>
  <div class="p-8">
    <h1 class="text-2xl font-bold mb-6">Escolha seu produto</h1>

    <section class="grid grid-cols-3 gap-4">
      <div v-for="product in products" :key="product.id" class="bg-white rounded-lg shadow p-4">
        <div>
          <p class="font-bold">{{ product.name }}</p>
          <p class="text-gray-500">R$ {{ product.price }}</p>
        </div>
        <button @click="handleBuy(product.id)" class="mt-4 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
          Comprar
        </button>
      </div>
      <p v-if="message" class="mb-4 text-green-500">{{ message }}</p>
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
    message.value = 'Pedido realizado com sucesso!'
  } catch (e) {
    message.value = 'Erro ao realizar pedido'
  }
}

</script>
