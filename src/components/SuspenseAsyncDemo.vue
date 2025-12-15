<script setup>
import { ref } from 'vue'

const delayMs = 900

await new Promise((resolve) => setTimeout(resolve, delayMs))

const loadedAt = new Date().toLocaleTimeString()
const message = ref(`Loaded after ${delayMs}ms at ${loadedAt}`)
</script>

<template>
  <div class="async-card">
    <h4>Async component (top-level await)</h4>
    <p class="msg">{{ message }}</p>
    <p class="hint">
      This component uses <code>await</code> at the top level inside <code>&lt;script setup&gt;</code>, so it becomes
      async and should be rendered under a <code>&lt;Suspense&gt;</code> boundary.
    </p>
  </div>
</template>

<style scoped>
.async-card {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 18px;
}

.async-card h4 {
  margin: 0 0 10px 0;
  color: #35495e;
}

.msg {
  margin: 0 0 10px 0;
  color: #2e7d32;
  font-weight: 800;
}

.hint {
  margin: 0;
  color: #607d8b;
  font-size: 13px;
}
</style>

