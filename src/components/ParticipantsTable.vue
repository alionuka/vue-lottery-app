<template>
  <table class="table custom-table">
    <thead>
      <tr>
        <th>#</th>
        <th>Name</th>
        <th>Date of Birth</th>
        <th>Email</th>
        <th>Phone number</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="(participant, index) in participants"
        :key="index"
        class="table-row"
      >
        <td>{{ index + 1 }}</td>
        <td>{{ participant.name }}</td>
        <td>{{ participant.birth }}</td>
        <td>{{ participant.email }}</td>
        <td>{{ participant.phone }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

interface Participant {
  name: string;
  birth: string;
  email: string;
  phone: string;
}

export default defineComponent({
  props: {
    participants: {
      type: Array as PropType<Participant[]>,
      required: true,
    },
  },
});
</script>

<style scoped>
.table {
  margin-top: 20px;
}

.custom-table {
  background-color: white; /* Білий фон для таблиці */
  border-collapse: collapse;
}

.table-row {
  background-color: white; /* Білий фон для рядків */
  color: #6c757d; /* Сірий текст */
}

.table-row td {
  color: #6c757d; /* Колір тексту всередині рядків */
  border-top: 1px solid #f1f1f1; /* Дуже світлий бордер між рядками */
}

.table th {
  color: #333; /* Трохи темніший колір для заголовків */
  font-weight: bold;
  border-bottom: 1px solid #e9ecef; /* Світліший бордер для заголовків */
}
</style>
