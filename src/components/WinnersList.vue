<template>
  <div class="winners-box p-2 d-flex align-items-center">
    <div
      class="d-flex flex-wrap gap-2 justify-content-center align-items-center"
    >
      <WinnerItem
        v-for="(winner, index) in winners"
        :key="index"
        :winner="winner"
        @remove="$emit('remove-winner', index)"
      />
    </div>
  </div>
</template>

<script setup>
import WinnerItem from "./WinnerItem.vue";

defineProps({
  winners: {
    type: Array,
    required: true,
  },
});

defineEmits(["remove-winner"]);
</script>

<style>
body {
  background: rgba(207, 207, 207, 0.2);
}
</style>
<style scoped>
.winners-box {
  border: 1px solid gray;
  width: 80%;
  min-height: 40px;
}
.winners-list {
  padding: 10px;
  border: 1px solid #dcdcdc; /* М'яка границя */
  border-radius: 8px; /* Заокруглені кути */
  background-color: #f9f9f9; /* Світлий фон */
  color: #6d6d6d;
}

/* Додатковий стиль для елементів списку */
.winner-item {
  margin-bottom: 8px;
  font-size: 16px;
}
</style>
