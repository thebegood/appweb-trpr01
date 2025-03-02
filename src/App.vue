<script setup lang="ts">
import BoxingList from "./components/BoxingList.vue"
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
</script>

<template>
  <div id="boxingList">
    <BoxingList />
  </div>
</template>

<style>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #a9b5df);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

#boxingList {
  background-color: #1b263b;
  border-radius: 8px;
}

body {
  margin: 0;
  display: flex;
  place-items: center;
  min-width: 320px;
  min-height: 100vh;
  background-color: #0d1b2a;
}

.product-save-animation {
  animation: pulseColor 0.5s ease forwards;
}

@keyframes pulseColor {
  0% {
    background-color: #3bd1ff;
  }
  50% {
    background-color: #4caf50;
  }
  100% {
    background-color: #778da9;
  }
}
</style>
