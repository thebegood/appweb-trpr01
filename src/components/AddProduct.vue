<script setup lang="ts">
import { ref, defineEmits } from "vue"
import type { Product } from "../types"

const emit = defineEmits(["add"])

const newProduct = ref<Product>({
  id: 0,
  title: "",
  description: "",
  color: "",
  stock: 0,
  price: 0,
})

function addProduct() {
  if (!newProduct.value.title.trim()) return

  emit("add", { ...newProduct.value, id: Date.now() })

  newProduct.value = {
    id: 0,
    title: "",
    description: "",
    color: "",
    stock: 0,
    price: 0,
  }
}
</script>

<template>
  <div class="input-group">
    <div class="input-item">
      <input v-model="newProduct.title" placeholder="Nom" />
    </div>
    <div class="input-item">
      <input v-model="newProduct.description" placeholder="Description" />
    </div>
    <div class="input-item">
      <input v-model="newProduct.color" placeholder="Couleur" />
    </div>
    <div class="input-item">
      <p>Qté:</p>
      <input v-model.number="newProduct.stock" placeholder="Quantité" />
    </div>
    <div class="input-item">
      <p>Prix:</p>
      <input v-model.number="newProduct.price" placeholder="Prix" />
      <p>$</p>
    </div>
    <button @click.prevent="addProduct" class="btn btn-primary">Ajouter</button>
  </div>
</template>

<style scoped>
.input-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
  text-align: center;
}

.input-item {
  margin-bottom: 0.5rem;
}

input,
select,
textarea {
  color: #e0e1dd;
  background: #415a77;
}

textarea:focus,
input:focus {
  color: #f3f3f3;
  background: #536375;
}
p {
  color: #e0e1dd;
}
</style>
