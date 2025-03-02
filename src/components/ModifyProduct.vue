<script setup lang="ts">
import { ref, defineProps, defineEmits, watch } from "vue"
import type { Product } from "../types"

const props = defineProps<{ product: Product }>()
const emit = defineEmits(["save", "cancel"])

//const modifiedProduct = ref<Product>({ ...props.product })

const modifiedProductName = ref<string>("")
const modifiedProductDescription = ref<string>("")
const modifiedProductColor = ref<string>("")
const modifiedProductStock = ref<number>(0)
const modifiedProductPrice = ref<number>(0)

watch(
  () => props.product,
  (newProduct) => {
    if (newProduct) {
      modifiedProductName.value = newProduct.title
      modifiedProductDescription.value = newProduct.description
      modifiedProductColor.value = newProduct.color
      modifiedProductStock.value = newProduct.stock
      modifiedProductPrice.value = newProduct.price
    }
  },
  { immediate: true } //https://vuejs.org/guide/essentials/watchers.html
)

const saveChanges = () => {
  if (!props.product) return

  emit("save", {
    id: props.product.id,
    title: modifiedProductName.value,
    description: modifiedProductDescription.value,
    color: modifiedProductColor.value,
    stock: modifiedProductStock.value,
    price: modifiedProductPrice.value,
  })
}

const cancelEdit = () => {
  emit("cancel")
}
</script>

<template>
  <form @submit.prevent="saveChanges">
    <div>
      <h2>Modification du produit</h2>

      <div class="input-group">
        <input
          class="input-item"
          v-model="modifiedProductName"
          placeholder="Modifier le nom du produit"
        />
        <input
          class="input-item"
          v-model="modifiedProductDescription"
          placeholder="Modifier la description du produit"
        />
        <input
          class="input-item"
          v-model="modifiedProductColor"
          placeholder="Modifier la couleur du produit"
        />
        <input
          class="input-item"
          v-model="modifiedProductStock"
          placeholder="Modifier la quantité du produit"
        />
        <input
          class="input-item"
          v-model="modifiedProductPrice"
          placeholder="Modifier le prix du produit"
        />

        <button type="submit" class="btn btn-success">Enregistrer</button>
        <button @click.prevent="cancelEdit" class="btn btn-secondary">
          Annuler
        </button>
      </div>
    </div>
  </form>
</template>
<style scoped>
h2 {
  color: #e0e1dd;
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
</style>
