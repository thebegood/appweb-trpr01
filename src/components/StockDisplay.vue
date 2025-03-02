<script setup lang="ts">
import { computed } from "vue"

const props = defineProps<{
  stock: number
}>()

const stockColor = computed(() => {
  if (props.stock <= 0) return "red"
  if (props.stock < 5) return "orange"
  return "green"
})
</script>

<template>
  <span :style="{ color: stockColor }">
    {{ stock }}
  </span>
</template>
