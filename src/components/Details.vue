<script setup lang="ts">
const props = defineProps<{
  product: {
    id: number
    title: string
    description: string
    color: string
  }
  showDetails: boolean
}>()
</script>

<template>
  <div>
    <div v-if="props.showDetails" class="details-content">
      <p>{{ props.product.description }}</p>
      <p>Couleur: {{ props.product.color }}</p>
    </div>
  </div>
</template>

<style scoped>
.details-content {
  margin-top: 10px;
}

p {
  color: #e0e1dd;
}
</style>
