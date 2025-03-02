<script setup lang="ts">
import { ref } from "vue"

const searchQuery = ref<string>("")

defineProps<{
  modelValue: string
}>()

const emit = defineEmits(["update:modelValue"])

function handleSearch(event: Event) {
  const query = (event.target as HTMLInputElement).value
  searchQuery.value = query
  emit("update:modelValue", query)
}
</script>

<template>
  <div class="input-group">
    <div class="input-item">
      <p>Rechercher par nom:</p>
      <input
        :value="searchQuery"
        @input="handleSearch"
        placeholder="Rechercher un produit..."
      />
    </div>
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

input {
  color: #e0e1dd;
  background: #415a77;
}

input:focus {
  color: #f3f3f3;
  background: #536375;
}
</style>
