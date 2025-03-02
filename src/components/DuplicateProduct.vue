<script setup lang="ts">
const props = defineProps<{
  productId: number
}>()

const emit = defineEmits(["duplicate"])

function confirmDuplicate() {
  emit("duplicate", props.productId)
}
</script>

<template>
  <button @click="confirmDuplicate" class="btn btn-secondary">Dupliquer</button>
</template>
