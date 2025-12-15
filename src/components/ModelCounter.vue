<script setup>
/**
 * ModelCounter.vue
 * Child component demonstrating defineModel for v-model binding
 * Vue 3.4+ feature - drastically simplifies two-way binding!
 */

// This single line replaces defineProps + defineEmits for v-model!
const model = defineModel()

// For TypeScript, you can add type:
// const model = defineModel<number>()

const increment = () => {
  model.value++
}

const decrement = () => {
  model.value--
}
</script>

<template>
  <div class="model-counter">
    <button @click="decrement" class="btn">-</button>
    <span class="value">{{ model }}</span>
    <button @click="increment" class="btn">+</button>
  </div>
</template>

<style scoped>
.model-counter {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
}

.btn {
  width: 44px;
  height: 44px;
  border: none;
  border-radius: 50%;
  background: #42b883;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn:hover {
  background: #3aa876;
  transform: scale(1.1);
}

.btn:active {
  transform: scale(0.95);
}

.value {
  font-size: 48px;
  font-weight: bold;
  color: #35495e;
  min-width: 80px;
  text-align: center;
}
</style>

