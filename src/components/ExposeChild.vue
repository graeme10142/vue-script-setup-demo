<script setup>
import { ref } from 'vue'

const count = ref(0)

const increment = () => {
  count.value++
}

const reset = () => {
  count.value = 0
}

defineExpose({
  count,
  increment,
  reset
})
</script>

<template>
  <div class="child">
    <p class="title">Child component</p>
    <p class="value">count: <strong>{{ count }}</strong></p>
    <div class="buttons">
      <button class="btn" @click="increment">Increment (child)</button>
      <button class="btn secondary" @click="reset">Reset (child)</button>
    </div>
    <p class="hint">
      This component exposes <code>{ count, increment, reset }</code> via <code>defineExpose</code>.
    </p>
  </div>
</template>

<style scoped>
.child {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 18px;
}

.title {
  margin: 0 0 8px 0;
  color: #35495e;
  font-weight: 800;
}

.value {
  margin: 0 0 14px 0;
  color: #455a64;
}

.buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-bottom: 12px;
}

.btn {
  padding: 10px 14px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  background: #42b883;
  color: white;
  font-weight: 700;
}

.btn.secondary {
  background: #35495e;
}

.hint {
  margin: 0;
  font-size: 13px;
  color: #607d8b;
}
</style>

