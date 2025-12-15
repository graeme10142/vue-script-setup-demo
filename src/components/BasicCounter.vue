<script setup>
/**
 * BasicCounter.vue
 * Demonstrates: Script setup basics - no return statement needed
 */
import { ref, computed } from 'vue'

// These are automatically available in the template
const count = ref(0)
const doubled = computed(() => count.value * 2)

const increment = () => {
  count.value++
}

const decrement = () => {
  count.value--
}

const reset = () => {
  count.value = 0
}
</script>

<template>
  <div class="counter">
    <div class="counter-display">
      <span class="count">{{ count }}</span>
      <span class="doubled">Doubled: {{ doubled }}</span>
    </div>

    <div class="counter-buttons">
      <button @click="decrement" class="btn btn-secondary">-</button>
      <button @click="reset" class="btn btn-outline">Reset</button>
      <button @click="increment" class="btn btn-primary">+</button>
    </div>

    <div class="code-note">
      <code>const count = ref(0)</code> -> No return statement needed!
    </div>
  </div>
</template>

<style scoped>
.counter {
  text-align: center;
  padding: 20px;
}

.counter-display {
  margin-bottom: 20px;
}

.count {
  display: block;
  font-size: 64px;
  font-weight: bold;
  color: #42b883;
}

.doubled {
  color: #666;
  font-size: 18px;
}

.counter-buttons {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

.btn {
  padding: 12px 24px;
  font-size: 18px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.btn-primary {
  background: #42b883;
  color: white;
}

.btn-primary:hover {
  background: #3aa876;
}

.btn-secondary {
  background: #35495e;
  color: white;
}

.btn-secondary:hover {
  background: #2c3e50;
}

.btn-outline {
  background: white;
  border: 2px solid #ddd;
  color: #666;
}

.btn-outline:hover {
  border-color: #42b883;
}

.code-note {
  background: #f5f5f5;
  padding: 12px;
  border-radius: 6px;
  font-size: 14px;
}

.code-note code {
  background: #e8e8e8;
  padding: 2px 6px;
  border-radius: 4px;
}
</style>

