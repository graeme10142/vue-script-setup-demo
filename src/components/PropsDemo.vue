<script setup>
/**
 * PropsDemo.vue
 * Demonstrates: defineProps with TypeScript-style declarations
 * Note: Vue 3.5+ allows reactive destructuring!
 */

// Method 1: Runtime declaration
// const props = defineProps({
//   title: String,
//   count: { type: Number, default: 0 }
// })

// Method 2: Type-based declaration with defaults (recommended)
const props = withDefaults(defineProps(), {
  title: 'Default Title',
  count: 0,
  items: () => []
})

// Vue 3.5+: We can now destructure and keep reactivity!
// const { title, count, items } = defineProps<Props>()
</script>

<template>
  <div class="props-demo">
    <div class="prop-card">
      <h3>Title Prop</h3>
      <p class="prop-value">{{ props.title }}</p>
      <code>title: string</code>
    </div>

    <div class="prop-card">
      <h3>Count Prop</h3>
      <p class="prop-value number">{{ props.count }}</p>
      <code>count?: number (default: 0)</code>
    </div>

    <div class="prop-card">
      <h3>Items Prop</h3>
      <ul class="items-list">
        <li v-for="(item, index) in props.items" :key="index">{{ item }}</li>
      </ul>
      <code>items?: string[] (default: [])</code>
    </div>

    <div class="code-block">
      <pre>// TypeScript-style props declaration
const props = withDefaults(defineProps&lt;{
  title: string
  count?: number
  items?: string[]
}&gt;(), {
  count: 0,
  items: () => []
})</pre>
    </div>
  </div>
</template>

<style scoped>
.props-demo {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}

.prop-card {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
}

.prop-card h3 {
  color: #35495e;
  font-size: 14px;
  margin-bottom: 10px;
}

.prop-value {
  font-size: 18px;
  font-weight: 600;
  color: #42b883;
  margin-bottom: 10px;
  word-break: break-word;
}

.prop-value.number {
  font-size: 32px;
}

.prop-card code {
  font-size: 11px;
  color: #666;
  background: #e9ecef;
  padding: 4px 8px;
  border-radius: 4px;
}

.items-list {
  list-style: none;
  padding: 0;
  margin-bottom: 10px;
}

.items-list li {
  background: #42b883;
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  display: inline-block;
  margin: 2px;
  font-size: 14px;
}

.code-block {
  grid-column: 1 / -1;
  background: #2d2d2d;
  color: #f8f8f2;
  padding: 16px;
  border-radius: 8px;
  overflow-x: auto;
}

.code-block pre {
  margin: 0;
  font-size: 13px;
  line-height: 1.5;
}
</style>
