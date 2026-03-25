<template>
  <div class="min-h-screen bg-brand-dark p-8 pb-20 flex flex-col items-center">

    <header class="w-full max-w-2xl mb-12 text-center">
      <h1 class="text-4xl font-black text-brand-pink-pale tracking-tighter">
        New <span class="text-brand-pink">Inventory</span> Item
      </h1>
      <p class="text-brand-pink-pale/40 font-medium">Add high-performance products to your catalog</p>
    </header>

    <section class="w-full max-w-2xl">
      <form @submit.prevent="handleCreateNewProduct"
        class="bg-brand-purple-dark/10 backdrop-blur-xl border border-brand-purple-dark/30 p-10 rounded-[2.5rem] shadow-2xl flex flex-col gap-6">

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="flex flex-col gap-2 md:col-span-2">
            <label class="text-xs font-bold uppercase tracking-[0.15em] text-brand-pink/60 ml-1">Product Name</label>
            <input v-model="name" type="text" placeholder="e.g. Quantum Processor X1"
              class="bg-brand-dark/40 border border-brand-purple-dark/50 text-white p-4 rounded-2xl outline-none focus:border-brand-pink focus:ring-1 focus:ring-brand-pink/30 transition-all placeholder:text-white/10"
              required />
          </div>

          <div class="flex flex-col gap-2">
            <label class="text-xs font-bold uppercase tracking-[0.15em] text-brand-pink/60 ml-1">Price (USD)</label>
            <div class="relative">
              <span class="absolute left-4 top-1/2 -translate-y-1/2 text-brand-pink font-bold">$</span>
              <input v-model="price" type="number" step="0.01" placeholder="0.00"
                class="w-full bg-brand-dark/40 border border-brand-purple-dark/50 text-white p-4 pl-8 rounded-2xl outline-none focus:border-brand-pink focus:ring-1 focus:ring-brand-pink/30 transition-all"
                required />
            </div>
          </div>

          <div class="flex flex-col gap-2">
            <label class="text-xs font-bold uppercase tracking-[0.15em] text-brand-pink/60 ml-1">Stock Quantity</label>
            <input v-model="stock" type="number" placeholder="0"
              class="bg-brand-dark/40 border border-brand-purple-dark/50 text-white p-4 rounded-2xl outline-none focus:border-brand-pink focus:ring-1 focus:ring-brand-pink/30 transition-all"
              required />
          </div>

          <div class="flex flex-col gap-2 md:col-span-2">
            <label class="text-xs font-bold uppercase tracking-[0.15em] text-brand-pink/60 ml-1">Category ID</label>
            <input v-model="category_id" type="number" placeholder="Select ID"
              class="bg-brand-dark/40 border border-brand-purple-dark/50 text-white p-4 rounded-2xl outline-none focus:border-brand-pink focus:ring-1 focus:ring-brand-pink/30 transition-all"
              required />
          </div>
        </div>

        <div class="flex flex-col gap-2">
          <label class="text-xs font-bold uppercase tracking-[0.15em] text-brand-pink/60 ml-1">Product Visual</label>
          <label
            class="group cursor-pointer relative flex flex-col items-center justify-center border-2 border-dashed border-brand-purple-dark/50 bg-brand-dark/20 p-8 rounded-3xl hover:border-brand-pink/50 hover:bg-brand-pink/5 transition-all">
            <input type="file" @change="handleFileUpload" accept="image/*" class="hidden" />

            <div class="text-center">
              <span class="text-3xl mb-2 block group-hover:scale-110 transition-transform">🖼</span>
              <p class="text-brand-pink-pale font-bold text-sm">
                {{ imageFile ? imageFile.name : 'Click to upload image' }}
              </p>
              <p class="text-brand-pink-pale/30 text-[10px] mt-1 uppercase tracking-widest">PNG, JPG or WEBP (Max 5MB)
              </p>
            </div>
          </label>
        </div>

        <button type="submit"
          class="group relative bg-brand-pink text-brand-dark font-black p-5 rounded-2xl mt-4 overflow-hidden transition-all hover:scale-[1.02] active:scale-[0.98] shadow-xl shadow-brand-pink/10">
          <div
            class="absolute inset-0 bg-white/20 translate-y-full group-hover:translate-y-0 transition-transform duration-300">
          </div>
          <span class="relative flex items-center justify-center gap-2">
            Deploy New Product
            <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="none" viewBox="0 0 24 24"
              stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M12 4v16m8-8H4" />
            </svg>
          </span>
        </button>
      </form>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import api from '@/services/api'

const name = ref('')
const price = ref('')
const stock = ref('')
const category_id = ref('')
const imageFile = ref<File | null>(null)

const handleFileUpload = (event: Event) => {
  const target = event.target as HTMLInputElement
  if (target.files && target.files[0]) {
    imageFile.value = target.files[0]
  }
}

const handleCreateNewProduct = async () => {
  try {
    const formData = new FormData()
    formData.append('product[name]', name.value)
    formData.append('product[price]', price.value)
    formData.append('product[stock]', stock.value)
    formData.append('product[category_id]', category_id.value)

    if (imageFile.value) {
      formData.append('product[image]', imageFile.value)
    }

    await api.post('/products', formData)

    alert('Product deployed to nexus successfully!')

    name.value = ''; price.value = ''; stock.value = '';
    category_id.value = ''; imageFile.value = null

  } catch (error) {
    alert('Deployment failed. Check system logs.')
  }
}
</script>
