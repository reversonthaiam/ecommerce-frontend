<template>
  <div class="p-8">
    <h1 class="text-2xl font-bold mb-6">Create New Product</h1>

    <section class="max-w-md">
      <form @submit.prevent="handleCreateNewProduct" class="flex flex-col gap-4">
        <div class="flex flex-col gap-1">
          <label class="font-medium">Product Name</label>
          <input v-model="name" type="text" placeholder="e.g. Wireless Mouse"
            class="border p-2 rounded focus:ring-2 focus:ring-blue-300 outline-none" required />
        </div>

        <div class="flex flex-col gap-1">
          <label class="font-medium">Price</label>
          <input v-model="price" type="number" step="0.01" placeholder="0.00"
            class="border p-2 rounded focus:ring-2 focus:ring-blue-300 outline-none" required />
        </div>

        <div class="flex flex-col gap-1">
          <label class="font-medium">Stock Quantity</label>
          <input v-model="stock" type="number" placeholder="0"
            class="border p-2 rounded focus:ring-2 focus:ring-blue-300 outline-none" required />
        </div>

        <div class="flex flex-col gap-1">
          <label class="font-medium">Category ID</label>
          <input v-model="category_id" type="number" placeholder="1"
            class="border p-2 rounded focus:ring-2 focus:ring-blue-300 outline-none" required />
        </div>

        <div class="flex flex-col gap-1">
          <label class="font-medium">Product Image</label>
          <input type="file" @change="handleFileUpload" accept="image/*" class="border p-1 rounded cursor-pointer" />
        </div>

        <button type="submit"
          class="bg-blue-500 text-white font-bold p-3 rounded mt-4 hover:bg-blue-600 transition-colors shadow-md">
          Create Product
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

const products = ref([])

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

    const response = await api.post('/products', formData)

    console.log('Produto criado com sucesso:', response.data)
    alert('Product created!')

    name.value = ''
    price.value = ''
    stock.value = ''
    category_id.value = ''
    imageFile.value = null

  } catch (error) {
    console.error('Error creating product.', error)
    alert('Error creating product.')
  }
}
</script>
