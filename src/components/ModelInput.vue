<script setup>
/**
 * ModelInput.vue
 * A reusable input component using defineModel
 */

// defineModel for two-way binding
const model = defineModel()

// Additional props for customization
defineProps({
  label: {
    type: String,
    default: ''
  },
  placeholder: {
    type: String,
    default: ''
  }
})
</script>

<template>
  <div class="model-input">
    <label v-if="label">{{ label }}</label>
    <input
      v-model="model"
      type="text"
      :placeholder="placeholder"
    />
  </div>
</template>

<style scoped>
.model-input {
  margin-bottom: 12px;
}

label {
  display: block;
  font-size: 13px;
  font-weight: 600;
  color: #666;
  margin-bottom: 6px;
}

input {
  width: 100%;
  padding: 10px 14px;
  border: 2px solid #e0e0e0;
  border-radius: 6px;
  font-size: 15px;
  transition: border-color 0.2s;
}

input:focus {
  outline: none;
  border-color: #42b883;
}

input::placeholder {
  color: #aaa;
}
</style>
