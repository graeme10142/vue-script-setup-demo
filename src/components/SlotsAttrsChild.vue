<script setup>
import { computed, useAttrs, useSlots } from 'vue'

defineOptions({
  inheritAttrs: false
})

const slots = useSlots()
const attrs = useAttrs()

const hasHeader = computed(() => !!slots.header)
const hasFooter = computed(() => !!slots.footer)

const attrEntries = computed(() =>
  Object.entries(attrs).map(([key, value]) => ({
    key,
    value: typeof value === 'string' ? value : JSON.stringify(value)
  }))
)
</script>

<template>
  <section class="card">
    <header class="card-header">
      <div class="badge">Child</div>
      <div class="meta">
        <div><strong>useSlots</strong>: header={{ hasHeader }}, footer={{ hasFooter }}</div>
        <div><strong>useAttrs</strong>: {{ attrEntries.length }} attrs received</div>
      </div>
    </header>

    <div class="forwarded" v-bind="attrs">
      <p class="label">This box has <code>v-bind="attrs"</code> applied.</p>
      <p class="small">Try passing <code>id</code>, <code>class</code>, <code>data-*</code>, or <code>title</code> from the parent.</p>
    </div>

    <div class="slots">
      <div class="slot">
        <div class="slot-title">Header slot</div>
        <div class="slot-body">
          <slot name="header">
            <em>(no header slot provided)</em>
          </slot>
        </div>
      </div>

      <div class="slot">
        <div class="slot-title">Default slot</div>
        <div class="slot-body">
          <slot />
        </div>
      </div>

      <div class="slot">
        <div class="slot-title">Footer slot</div>
        <div class="slot-body">
          <slot name="footer">
            <em>(no footer slot provided)</em>
          </slot>
        </div>
      </div>
    </div>

    <details class="attrs">
      <summary>View received attrs</summary>
      <ul>
        <li v-for="entry in attrEntries" :key="entry.key">
          <code>{{ entry.key }}</code>: <span class="value">{{ entry.value }}</span>
        </li>
      </ul>
    </details>
  </section>
</template>

<style scoped>
.card {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 18px;
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.card-header {
  display: flex;
  gap: 12px;
  align-items: flex-start;
}

.badge {
  background: #35495e;
  color: white;
  font-size: 12px;
  font-weight: 700;
  padding: 6px 10px;
  border-radius: 999px;
}

.meta {
  color: #455a64;
  font-size: 13px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.forwarded {
  border: 2px dashed #90a4ae;
  border-radius: 10px;
  padding: 12px 14px;
  background: white;
}

.label {
  margin: 0 0 6px 0;
  font-size: 14px;
  color: #263238;
}

.small {
  margin: 0;
  font-size: 12px;
  color: #607d8b;
}

.slots {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 12px;
}

.slot {
  background: white;
  border-radius: 10px;
  border: 1px solid #e0e0e0;
  overflow: hidden;
}

.slot-title {
  background: #e8f5e9;
  color: #2e7d32;
  font-weight: 700;
  padding: 10px 12px;
  font-size: 12px;
}

.slot-body {
  padding: 12px;
  color: #455a64;
  font-size: 13px;
}

.attrs {
  background: #263238;
  color: #eceff1;
  border-radius: 10px;
  padding: 12px 14px;
}

.attrs summary {
  cursor: pointer;
  font-weight: 700;
}

.attrs ul {
  margin: 12px 0 0 0;
  padding-left: 18px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.value {
  color: #80cbc4;
  word-break: break-word;
}
</style>

