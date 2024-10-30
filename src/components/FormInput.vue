<template>
  <div class="form-group mb-3">
    <label :for="id" class="form-label">{{ label }}</label>
    <input
      :type="type"
      :id="id"
      class="form-control custom-input"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      :placeholder="placeholder"
      :class="{ 'is-invalid': error }"
    />
    <div v-if="error" class="invalid-feedback">{{ error }}</div>
  </div>
</template>

<script setup>
defineProps({
  id: {
    type: String,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
  modelValue: {
    type: [String, Number],
    default: "",
  },
  type: {
    type: String,
    default: "text",
  },
  placeholder: {
    type: String,
    default: "",
  },
  error: {
    type: String,
    default: "",
  },
});

defineEmits(["update:modelValue"]);
</script>

<style scoped>
.custom-input {
  background-color: rgba(223, 223, 223, 0.37);
  padding: 8px;
  font-size: 16px;
  border-radius: 4px;
  border: 1px solid #ced4da;
  color: #333;
}

/* Стан фокусу (при натисканні) */
.custom-input:focus {
  border-color: #007bff;
  outline: none;
}

/* Стилі для відображення помилок */
.is-invalid {
  border-color: red;
}

.invalid-feedback {
  color: red;
  font-size: 12px;
}
</style>
