<script setup lang="ts">
import { ref, watch } from "vue"
import type { Product } from "../types"

const props = defineProps<{
  stock: number
  product: Product
}>()

const showAlert = ref(false)

watch(
  () => props.stock,
  (newStock) => {
    if (newStock <= 0) {
      showAlert.value = true
      setTimeout(() => {
        showAlert.value = false
      }, 5000)
    }
  },
  { immediate: true } //https://vuejs.org/guide/essentials/watchers.html
)
</script>

<template>
  <div v-if="showAlert" class="stock-alert">
    <p>Le produit "{{ props.product.title }}" est en rupture de stock.</p>
  </div>
</template>

<style scoped>
.stock-alert {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #f43636;
  color: white;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 1000;
}

.stock-alert p {
  margin: 0;
}
</style>
