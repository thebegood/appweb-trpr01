<script setup lang="ts">
import { defineProps } from "vue"

//aide de : https://stackoverflow.com/questions/14964035/how-to-export-javascript-array-info-to-csv-on-client-side?page=1&tab=scoredesc#tab-top
const props = defineProps<{
  data: Array<{
    id: number
    title: string
    description: string
    color: string
    stock: number
    price: number
  }>
  filename?: string
}>()

function exportToCsv() {
  const headers = ["ID", "Nom", "Description", "Couleur", "Quantité", "Prix"]

  const rows = props.data.map((item) => [
    item.id,
    `"${item.title}"`,
    `"${item.description}"`,
    `"${item.color}"`,
    item.stock,
    item.price,
  ])

  const csvContent =
    "data:text/csv;charset=utf-8," +
    [headers, ...rows].map((row) => row.join(",")).join("\n")

  const encodedUri = encodeURI(csvContent)
  const link = document.createElement("a")
  link.setAttribute("href", encodedUri)
  link.setAttribute("download", props.filename || "data.csv")
  document.body.appendChild(link)

  link.click()
  document.body.removeChild(link)
}
</script>

<template>
  <button @click="exportToCsv" class="export-button">Exporter en CSV</button>
</template>

<style scoped>
.export-button {
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.export-button:hover {
  background-color: #45a049;
}
</style>
