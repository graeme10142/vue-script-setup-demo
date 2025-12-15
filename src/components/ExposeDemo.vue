<script setup>
import { computed, ref } from 'vue'
import ExposeChild from './ExposeChild.vue'

const childRef = ref(null)

const exposedCount = computed(() => childRef.value?.count?.value ?? null)

const callIncrement = () => {
  childRef.value?.increment?.()
}

const callReset = () => {
  childRef.value?.reset?.()
}
</script>

<template>
  <div class="demo">
    <ExposeChild ref="childRef" />

    <div class="panel">
      <div class="row">
        <strong>Parent sees (via template ref):</strong>
        <code v-if="exposedCount !== null">{{ exposedCount }}</code>
        <em v-else>(child not mounted)</em>
      </div>
      <div class="buttons">
        <button class="btn" @click="callIncrement">Call child.increment()</button>
        <button class="btn secondary" @click="callReset">Call child.reset()</button>
      </div>
      <p class="note">
        This works because the child uses <code>defineExpose</code>; otherwise, the parent would not be able to
        access internal refs and methods.
      </p>
    </div>
  </div>
</template>

<style scoped>
.demo {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

@media (max-width: 700px) {
  .demo {
    grid-template-columns: 1fr;
  }
}

.panel {
  background: #fff3e0;
  border-left: 4px solid #ff9800;
  border-radius: 12px;
  padding: 18px;
}

.row {
  display: flex;
  gap: 10px;
  align-items: center;
  color: #bf360c;
  margin-bottom: 12px;
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

.note {
  margin: 0;
  color: #bf360c;
  font-size: 13px;
}
</style>

