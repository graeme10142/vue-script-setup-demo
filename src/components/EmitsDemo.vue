<script setup>
/**
 * EmitsDemo.vue
 * Demonstrates: defineEmits for type-safe event emission
 */
import { ref } from 'vue'

// Define emits with TypeScript-style validation
const emit = defineEmits(['notify'])

// For full TypeScript support, use this syntax:
// const emit = defineEmits<{
//   (e: 'notify', message: string): void
//   (e: 'update', payload: { id: number; value: string }): void
// }>()

const message = ref('')
const eventLog = ref([])

const sendNotification = () => {
  if (message.value.trim()) {
    emit('notify', message.value)
    eventLog.value.unshift({
      time: new Date().toLocaleTimeString(),
      message: message.value
    })
    message.value = ''
  }
}

const quickNotify = (text) => {
  emit('notify', text)
  eventLog.value.unshift({
    time: new Date().toLocaleTimeString(),
    message: text
  })
}
</script>

<template>
  <div class="emits-demo">
    <div class="input-section">
      <input
        v-model="message"
        type="text"
        placeholder="Type a message..."
        @keyup.enter="sendNotification"
      />
      <button @click="sendNotification" class="btn-primary">
        Emit Event
      </button>
    </div>

    <div class="quick-buttons">
      <span>Quick emit:</span>
      <button @click="quickNotify('Hello!')">Hello!</button>
      <button @click="quickNotify('Vue 3 is awesome!')">Vue 3!</button>
      <button @click="quickNotify('Script setup rocks!')">Script Setup!</button>
    </div>

    <div class="event-log">
      <h4>Event Log <small>(events emitted to parent)</small></h4>
      <div v-if="eventLog.length === 0" class="empty">
        No events emitted yet. Click a button above!
      </div>
      <div v-for="(event, index) in eventLog" :key="index" class="log-entry">
        <span class="time">{{ event.time }}</span>
        <span class="msg">{{ event.message }}</span>
      </div>
    </div>

    <div class="code-block">
      <pre>// Type-safe emits declaration
const emit = defineEmits&lt;{
  (e: 'notify', message: string): void
}&gt;()

// Emit the event
emit('notify', 'Hello from child!')</pre>
    </div>
  </div>
</template>

<style scoped>
.emits-demo {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.input-section {
  display: flex;
  gap: 10px;
}

.input-section input {
  flex: 1;
  padding: 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.2s;
}

.input-section input:focus {
  outline: none;
  border-color: #42b883;
}

.btn-primary {
  padding: 12px 24px;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: background 0.2s;
}

.btn-primary:hover {
  background: #3aa876;
}

.quick-buttons {
  display: flex;
  gap: 10px;
  align-items: center;
  flex-wrap: wrap;
}

.quick-buttons span {
  color: #666;
  font-size: 14px;
}

.quick-buttons button {
  padding: 8px 16px;
  background: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.2s;
}

.quick-buttons button:hover {
  background: #e8f5e9;
  border-color: #42b883;
}

.event-log {
  background: #f8f9fa;
  border-radius: 8px;
  padding: 16px;
}

.event-log h4 {
  margin-bottom: 12px;
  color: #35495e;
}

.event-log h4 small {
  font-weight: normal;
  color: #999;
}

.empty {
  color: #999;
  font-style: italic;
  text-align: center;
  padding: 20px;
}

.log-entry {
  display: flex;
  gap: 12px;
  padding: 8px 12px;
  background: white;
  border-radius: 4px;
  margin-bottom: 6px;
}

.log-entry .time {
  color: #999;
  font-size: 12px;
  font-family: monospace;
}

.log-entry .msg {
  color: #42b883;
  font-weight: 500;
}

.code-block {
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
