<template>
  <div class="min-h-screen bg-brand-dark p-8 pb-20">
    <header class="mb-12">
      <h1 class="text-4xl font-black text-brand-pink-pale tracking-tighter">
        Choose your <span class="text-brand-pink">Product</span>
      </h1>
      <p class="text-brand-pink-pale/40 font-medium">Browse our premium selection</p>
    </header>

    <Transition name="fade">
      <div v-if="message"
        :class="message.includes('Error') ? 'border-red-500/50 text-red-400' : 'border-brand-pink/50 text-brand-pink'"
        class="mb-8 p-4 rounded-2xl bg-brand-purple-dark/10 border backdrop-blur-md font-bold text-center italic shadow-lg">
        {{ message }}
      </div>
    </Transition>

    <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
      <div v-for="product in products" :key="product.id"
        class="group relative bg-brand-purple-dark/5 backdrop-blur-md border border-brand-purple-dark/20 rounded-[2rem] p-5 transition-all duration-500 hover:bg-brand-purple-dark/20 hover:-translate-y-2 hover:shadow-2xl hover:shadow-brand-pink/10">

        <div class="relative overflow-hidden rounded-2xl mb-6 aspect-square">
          <img v-if="product.image_url" :src="product.image_url" :alt="product.name"
            class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" />

          <div v-else
            class="w-full h-full bg-brand-dark/40 flex flex-col items-center justify-center border border-brand-purple-dark/30">
            <span class="text-4xl mb-2">📦</span>
            <p class="text-brand-pink-pale/20 text-xs font-black uppercase tracking-widest">No Preview</p>
          </div>

          <div
            class="absolute top-3 right-3 bg-brand-dark/80 backdrop-blur-md text-brand-pink font-black px-4 py-2 rounded-xl text-sm border border-brand-pink/20">
            ${{ product.price }}
          </div>
        </div>

        <div class="space-y-1 px-1">
          <h3 class="text-xl font-bold text-brand-pink-pale group-hover:text-brand-pink transition-colors">
            {{ product.name }}
          </h3>
          <p class="text-brand-pink-pale/40 text-sm font-medium">Premium Hardware</p>
        </div>

        <button @click="handleBuy(product.id)"
          class="group/btn relative mt-6 w-full overflow-hidden bg-brand-purple-dark/40 border border-brand-pink/30 text-brand-pink-pale font-black py-4 rounded-2xl transition-all hover:bg-brand-pink hover:text-brand-dark hover:border-brand-pink active:scale-[0.95]">
          <span class="relative z-10 flex items-center justify-center gap-2">
            Buy Now
            <span
              class="opacity-0 group-hover/btn:opacity-100 transition-all translate-x-[-10px] group-hover/btn:translate-x-0">→</span>
          </span>
        </button>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import api from '@/services/api'
import type { Product } from '@/types/Product'

const products = ref<Product[]>([])
const message = ref('')

onMounted(async () => {
  try {
    const response = await api.get('/products')
    products.value = response.data
  } catch (e) {
    console.error("Failed to load products")
  }
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
    setTimeout(() => message.value = '', 3000)
  } catch (e) {
    message.value = 'Error placing order'
    setTimeout(() => message.value = '', 3000)
  }
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
