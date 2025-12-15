<script>
import { reactive } from 'vue'

// Runs once per module (shared across all component instances)
export const sharedState = reactive({ count: 0 })

export default {
  name: 'DualScriptDemo'
}
</script>

<script setup>
import { ref } from 'vue'

// Runs per component instance
const localCount = ref(0)

const incShared = () => {
  sharedState.count++
}

const incLocal = () => {
  localCount.value++
}

const resetLocal = () => {
  localCount.value = 0
}
</script>

<template>
  <div class="demo">
    <div class="card">
      <h4>Regular &lt;script&gt; (module scope)</h4>
      <p><strong>sharedState.count</strong>: <span class="value">{{ sharedState.count }}</span></p>
      <button class="btn" @click="incShared">Increment shared</button>
      <p class="hint">
        Because <code>sharedState</code> is created in the regular <code>&lt;script&gt;</code>, it is shared across
        every instance of this component.
      </p>
    </div>

    <div class="card">
      <h4>&lt;script setup&gt; (instance scope)</h4>
      <p><strong>localCount</strong>: <span class="value">{{ localCount }}</span></p>
      <div class="buttons">
        <button class="btn" @click="incLocal">Increment local</button>
        <button class="btn secondary" @click="resetLocal">Reset local</button>
      </div>
      <p class="hint">
        This state is per-instance and resets when the component is unmounted/remounted.
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

.card {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 18px;
}

.card h4 {
  margin: 0 0 10px 0;
  color: #35495e;
}

.value {
  color: #42b883;
  font-weight: 900;
  font-size: 18px;
}

.buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.btn {
  padding: 10px 14px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  background: #42b883;
  color: white;
  font-weight: 800;
}

.btn.secondary {
  background: #35495e;
}

.hint {
  margin: 10px 0 0 0;
  color: #607d8b;
  font-size: 13px;
}
</style>

