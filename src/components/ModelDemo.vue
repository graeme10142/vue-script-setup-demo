<script setup>
/**
 * ModelDemo.vue
 * Demonstrates: defineModel (Vue 3.4+) - simplified two-way binding
 * This is the parent component that uses child components with v-model
 */
import { ref } from 'vue'
import ModelCounter from './ModelCounter.vue'
import ModelInput from './ModelInput.vue'

// State that we'll bind to child components via v-model
const count = ref(0)
const username = ref('')
const email = ref('')
</script>

<template>
  <div class="model-demo">
    <div class="demo-grid">
      <!-- Counter with v-model -->
      <div class="demo-card">
        <h4>Counter Component</h4>
        <ModelCounter v-model="count" />
        <p class="parent-value">Parent sees: <strong>{{ count }}</strong></p>
      </div>

      <!-- Text inputs with v-model -->
      <div class="demo-card">
        <h4>Input Components</h4>
        <ModelInput v-model="username" label="Username" placeholder="Enter username" />
        <ModelInput v-model="email" label="Email" placeholder="Enter email" />
        <p class="parent-value">
          Parent sees: <strong>{{ username || '(empty)' }}</strong> / <strong>{{ email || '(empty)' }}</strong>
        </p>
      </div>
    </div>

    <div class="explanation">
      <h4>How defineModel Works</h4>
      <div class="comparison">
        <div class="old-way">
          <h5>Old Way (Vue 3.3 and earlier)</h5>
          <pre>// Child component
const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])

const updateValue = (val) => {
  emit('update:modelValue', val)
}</pre>
        </div>
        <div class="new-way">
          <h5>New Way (Vue 3.4+)</h5>
          <pre>// Child component
const model = defineModel()

// That's it! Use model.value directly
model.value = newValue</pre>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.model-demo {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.demo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 20px;
}

.demo-card {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 12px;
}

.demo-card h4 {
  color: #35495e;
  margin-bottom: 16px;
  padding-bottom: 8px;
  border-bottom: 2px solid #e0e0e0;
}

.parent-value {
  margin-top: 16px;
  padding: 12px;
  background: #e8f5e9;
  border-radius: 6px;
  font-size: 14px;
  color: #2e7d32;
}

.parent-value strong {
  color: #1b5e20;
}

.explanation {
  background: #fff3e0;
  padding: 20px;
  border-radius: 12px;
  border-left: 4px solid #ff9800;
}

.explanation h4 {
  color: #e65100;
  margin-bottom: 16px;
}

.comparison {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

@media (max-width: 600px) {
  .comparison {
    grid-template-columns: 1fr;
  }
}

.old-way,
.new-way {
  background: #2d2d2d;
  border-radius: 8px;
  overflow: hidden;
}

.old-way h5,
.new-way h5 {
  padding: 10px 16px;
  margin: 0;
  font-size: 13px;
}

.old-way h5 {
  background: #c62828;
  color: white;
}

.new-way h5 {
  background: #2e7d32;
  color: white;
}

.old-way pre,
.new-way pre {
  margin: 0;
  padding: 16px;
  font-size: 12px;
  line-height: 1.5;
  color: #f8f8f2;
  overflow-x: auto;
}
</style>

