<script setup lang="ts">
import { ref, computed } from "vue"
import type { Product } from "../types"
import Details from "./Details.vue"
import StockAlert from "./StockAlert.vue"
import StockDisplay from "./StockDisplay.vue"
import DeleteProduct from "./DeleteProduct.vue"
import DuplicateProduct from "./DuplicateProduct.vue"
const props = defineProps<{
  product: Product
  onDelete: (id: number) => void
  onEdit: (id: number) => void
  onDuplicate: (id: number) => void
}>()

const showDetails = ref(false)

const toggleDetails = () => {
  const productElement = document.getElementById(`product-${props.product.id}`)

  if (productElement) {
    productElement.classList.add("product-save-animation")

    setTimeout(() => {
      productElement.classList.remove("product-save-animation")
    }, 500)
  }

  showDetails.value = !showDetails.value
}

computed(() => {
  if (props.product.stock <= 0) return "red"
  if (props.product.stock < 5) return "orange"
  return "green"
})

const emit = defineEmits(["remove", "modify", "duplicate"])
</script>
<template>
  <li
    @click="toggleDetails"
    :id="'product-' + props.product.id"
    class="product-item"
  >
    <StockAlert :stock="props.product.stock" :product="product" />

    <div id="product">
      <h3>
        <StockDisplay :stock="props.product.stock" />
        {{ props.product.title }} - {{ props.product.price }}$
      </h3>
      <Details :product="props.product" :showDetails="showDetails" />

      <button @click="onEdit(props.product.id)" class="btn btn-primary">
        Modifier
      </button>

      <DuplicateProduct
        :product-id="props.product.id"
        @duplicate="onDuplicate"
      />
      <DeleteProduct :productId="props.product.id" @delete="onDelete" />
    </div>
  </li>
</template>
<style scoped>
ul {
  list-style-type: none;
}

li {
  background-color: #415a77;
  border-radius: 8px;
  margin: 20px;
  transition: background-color 0.5s ease;
}

h3 {
  color: #e9ebe6;
}

p {
  color: #e0e1dd;
}

li:hover {
  background-color: #778da9;
}

.product-item {
  transition: background-color 0.5s ease;
}
</style>
