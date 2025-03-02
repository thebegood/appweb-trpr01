<script setup lang="ts">
import { ref, computed } from "vue"
import type { Product } from "../types"
import ProductItem from "./ProductItem.vue"
import ModifyProduct from "./ModifyProduct.vue"
import logo from "../assets/boxLogo.png"
import SearchBar from "./SearchBar.vue"
import ExportToCsv from "./ExportToCsv.vue"

import AddProduct from "./AddProduct.vue"

const products = ref<Product[]>([
  {
    id: 1,
    title: "Shin pads",
    description:
      "These top of the range shin pads have undergone rigorous testing. So you can combat with confidence and train with maximum protection for your legs. Made from a lightweight material, this shin guard will not weigh you down. Built with a foam layer, this shin instep will dissipate shock before it gets anywhere near your shin bone. Featuring integrated gel for further protection and cushioning, moisture management lining to ventilate and keep your skin dry, plus a special elastic to give you the perfect fit. A lightweight leg armour that will keep you agile for opponents.",
    color: "Red",
    stock: 2,
    price: 30,
  },
])

const modifiedProduct = ref<Product | null>(null)

const addProduct = (newProduct: Product) => {
  if (!newProduct.title.trim()) return

  products.value.push({
    id: Date.now(),
    title: newProduct.title,
    description: newProduct.description,
    color: newProduct.color,
    stock: newProduct.stock,
    price: newProduct.price,
  })

  newProduct.title = ""
  newProduct.description = ""
  newProduct.color = ""
  newProduct.stock = 0
  newProduct.price = 0
}
const removeProduct = (id: number) => {
  products.value = products.value.filter((p) => p.id !== id)
}
const startModify = (id: number) => {
  modifiedProduct.value = products.value.find((p) => p.id === id) || null
}

const saveModifiedProduct = (updatedProduct: Product) => {
  const index = products.value.findIndex((p) => p.id === updatedProduct.id)
  if (index !== -1) {
    products.value[index] = { ...updatedProduct }
    products.value[index].stock = updatedProduct.stock
  }
  modifiedProduct.value = null

  //AIDE DE CHATGPT POUR L'ANIMATION
  const productElement = document.getElementById(`product-${updatedProduct.id}`)
  if (productElement) {
    productElement.classList.add("product-save-animation")

    setTimeout(() => {
      productElement.classList.remove("product-save-animation")
    }, 500)
  }
}
const cancelModification = () => {
  modifiedProduct.value = null
}
const duplicateProduct = (id: number) => {
  const product = products.value.find((p) => p.id === id)
  if (!product) return
  products.value.push({
    id: Date.now(),
    title: product.title,
    description: product.description,
    color: product.color,
    stock: product.stock,
    price: product.price,
  })
}

const searchQuery = ref<string>("")

const filteredProducts = computed(() => {
  if (!searchQuery.value.trim()) {
    return products.value
  }
  return products.value.filter((product) =>
    product.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <!-- DEMANDER QUEL FORM ACTION DONNER-->
  <form @submit.prevent>
    <div class="container">
      <div
        :class="['left-section', { 'left-section-centered': !modifiedProduct }]"
      >
        <div class="title-container">
          <img :src="logo" alt="Boxing Logo" class="logo" />

          <h2>Mes produits de boxe</h2>
        </div>

        <ExportToCsv
          id="ExportToCsv"
          :data="products"
          filename="boxing_products.csv"
        />

        <!-- Ajout de produit-->

        <div class="input-group">
          <p>Produit:</p>

          <AddProduct @add="addProduct" />
        </div>

        <SearchBar v-model="searchQuery" />

        <!-- Liste Produits -->
        <ul v-if="filteredProducts.length > 0">
          <ProductItem
            v-for="product in filteredProducts"
            :key="product.id"
            :product="product"
            :onDelete="removeProduct"
            :onEdit="startModify"
            :onDuplicate="duplicateProduct"
          />
        </ul>

        <p v-else>Aucun produit dans la liste.</p>
      </div>

      <div
        :class="['right-section', { 'right-section-hidden': !modifiedProduct }]"
      >
        <!-- Modifier Product Component -->
        <ModifyProduct
          v-if="modifiedProduct"
          :product="modifiedProduct"
          @save="saveModifiedProduct"
          @cancel="cancelModification"
        />
      </div>
    </div>
  </form>
</template>
<style scoped>
.title-container {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo {
  width: 100px;
  height: auto;
}
h2 {
  color: #e0e1dd;
}

p {
  color: #e0e1dd;
}

.container {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.left-section {
  width: 48%;
  transition: width 0.3s ease; /* aide de chatgpt pour la transition fluide */
}
.left-section-centered {
  margin: 0 auto;
  width: 100%;
}
.right-section {
  width: 48%;
  transition: width 0.3s ease;
}

.right-section-hidden {
  width: 0;
  visibility: hidden;
}
.input-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
  text-align: center;
}

.input-item {
  margin-bottom: 0.5rem;
}

button #ExportToCsv {
  text-align: center;
  display: block;
  border-radius: 10px;
  transition: transform 0.2s ease, background-color 0.2s ease;
}

button:active {
  transform: scale(1.1);
  background-color: #4caf50;
}
</style>
