<template>
  <div class="winners-block">
    <h2>Переможці</h2>
    <ul>
      <li v-for="winner in winners" :key="winner.id">
        {{ winner.name }} — Виграш: {{ winner.prize }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

// Імітація даних переможців
const winners = ref([
  { id: 1, name: "Іван Іванов", prize: "1000$" },
  { id: 2, name: "Марія Петренко", prize: "500$" },
  { id: 3, name: "Олена Коваленко", prize: "300$" },
]);
</script>

<style scoped>
.winners-block {
  margin: 20px;
  padding: 10px;
  border: 1px solid #ccc;
}
</style>
