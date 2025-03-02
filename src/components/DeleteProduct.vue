<script setup lang="ts">
const props = defineProps<{
  productId: number
}>()

const emit = defineEmits(["delete"])

function confirmDelete() {
  if (confirm("Êtes-vous sûr de vouloir supprimer ce produit ?")) {
    emit("delete", props.productId)
  }
}
</script>

<template>
  <button @click="confirmDelete" class="btn btn-danger">Retirer</button>
</template>
