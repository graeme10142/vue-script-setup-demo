<script setup>
/**
 * ReactivityDemo.vue
 * Demonstrates: ref, reactive, computed, watch, watchEffect
 */
import { ref, reactive, computed, watch, watchEffect } from 'vue'

// ref - for primitive values
const searchQuery = ref('')

// reactive - for objects
const formData = reactive({
  firstName: '',
  lastName: '',
  age: null
})

// computed - derived values that auto-update
const fullName = computed(() => {
  if (!formData.firstName && !formData.lastName) return '(Enter a name)'
  return `${formData.firstName} ${formData.lastName}`.trim()
})

const isAdult = computed(() => {
  return formData.age !== null && formData.age >= 18
})

const searchResults = computed(() => {
  if (!searchQuery.value) return []
  const query = searchQuery.value.toLowerCase()
  const items = ['Vue.js', 'React', 'Angular', 'Svelte', 'Solid', 'Qwik', 'Astro']
  return items.filter((item) => item.toLowerCase().includes(query))
})

// Watch log for demonstration
const watchLog = ref([])

// watch - react to specific changes
watch(searchQuery, (newVal, oldVal) => {
  watchLog.value.unshift({
    type: 'watch',
    message: `Search changed: "${oldVal}" -> "${newVal}"`,
    time: new Date().toLocaleTimeString()
  })
  // Keep only last 5 entries
  if (watchLog.value.length > 5) watchLog.value.pop()
})

// watch can also watch reactive objects
watch(
  () => formData.firstName,
  (newVal) => {
    if (newVal) {
      watchLog.value.unshift({
        type: 'watch',
        message: `First name updated: "${newVal}"`,
        time: new Date().toLocaleTimeString()
      })
      if (watchLog.value.length > 5) watchLog.value.pop()
    }
  }
)

// watchEffect - auto-tracks dependencies
watchEffect(() => {
  if (formData.age !== null && formData.age > 0) {
    watchLog.value.unshift({
      type: 'watchEffect',
      message: `Age is now ${formData.age} (${isAdult.value ? 'adult' : 'minor'})`,
      time: new Date().toLocaleTimeString()
    })
    if (watchLog.value.length > 5) watchLog.value.pop()
  }
})
</script>

<template>
  <div class="reactivity-demo">
    <div class="demo-grid">
      <!-- ref demo -->
      <div class="demo-card">
        <h4>ref + computed</h4>
        <input v-model="searchQuery" type="text" placeholder="Search frameworks..." class="search-input" />
        <div class="results">
          <span v-if="searchResults.length === 0 && searchQuery" class="no-results"> No matches </span>
          <span v-for="result in searchResults" :key="result" class="result-tag">
            {{ result }}
          </span>
        </div>
        <code>const searchQuery = ref('')</code>
      </div>

      <!-- reactive demo -->
      <div class="demo-card">
        <h4>reactive + computed</h4>
        <input v-model="formData.firstName" placeholder="First name" />
        <input v-model="formData.lastName" placeholder="Last name" />
        <input v-model.number="formData.age" type="number" placeholder="Age" />

        <div class="computed-results">
          <p>
            Full name: <strong>{{ fullName }}</strong>
          </p>
          <p>
            Status:
            <span :class="isAdult ? 'adult' : 'minor'">
              {{ isAdult ? 'Adult' : 'Minor' }}
            </span>
          </p>
        </div>
        <code>const formData = reactive({ ... })</code>
      </div>
    </div>

    <!-- Watch log -->
    <div class="watch-log">
      <h4>Watch Log <small>(watch & watchEffect triggers)</small></h4>
      <div v-if="watchLog.length === 0" class="empty">Type in the inputs above to see watch triggers</div>
      <div v-for="(entry, i) in watchLog" :key="i" class="log-entry">
        <span :class="['badge', entry.type]">{{ entry.type }}</span>
        <span class="message">{{ entry.message }}</span>
        <span class="time">{{ entry.time }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.reactivity-demo {
  display: flex;
  flex-direction: column;
  gap: 20px;
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
}

.demo-card input {
  width: 100%;
  padding: 10px 14px;
  border: 2px solid #e0e0e0;
  border-radius: 6px;
  font-size: 15px;
  margin-bottom: 10px;
  transition: border-color 0.2s;
}

.demo-card input:focus {
  outline: none;
  border-color: #42b883;
}

.search-input {
  margin-bottom: 12px !important;
}

.results {
  min-height: 40px;
  margin-bottom: 12px;
}

.result-tag {
  display: inline-block;
  background: #42b883;
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 13px;
  margin: 2px;
}

.no-results {
  color: #999;
  font-style: italic;
}

.computed-results {
  background: white;
  padding: 12px;
  border-radius: 6px;
  margin-bottom: 12px;
}

.computed-results p {
  margin: 6px 0;
  font-size: 14px;
}

.adult {
  color: #2e7d32;
  font-weight: 600;
}

.minor {
  color: #f57c00;
}

.demo-card code {
  display: block;
  background: #e9ecef;
  padding: 8px 12px;
  border-radius: 4px;
  font-size: 12px;
  color: #666;
}

.watch-log {
  background: #263238;
  padding: 20px;
  border-radius: 12px;
  color: white;
}

.watch-log h4 {
  margin-bottom: 12px;
}

.watch-log h4 small {
  font-weight: normal;
  opacity: 0.6;
}

.empty {
  opacity: 0.5;
  font-style: italic;
  text-align: center;
  padding: 20px;
}

.log-entry {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  font-size: 13px;
}

.badge {
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
}

.badge.watch {
  background: #7c4dff;
}

.badge.watchEffect {
  background: #00bcd4;
}

.message {
  flex: 1;
  color: #b0bec5;
}

.time {
  font-family: monospace;
  opacity: 0.5;
  font-size: 11px;
}
</style>

