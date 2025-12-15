<script setup>
import { computed, ref } from 'vue'
import SlotsAttrsChild from './SlotsAttrsChild.vue'

const includeHeader = ref(true)
const includeFooter = ref(false)

const parentId = ref('slots-attrs-demo')
const parentTitle = ref('Hover me (title attr from parent)')
const parentDataTag = ref('from-parent')

const passthroughAttrs = computed(() => ({
  id: parentId.value,
  title: parentTitle.value,
  'data-tag': parentDataTag.value
}))
</script>

<template>
  <div class="demo">
    <div class="controls">
      <label class="toggle">
        <input v-model="includeHeader" type="checkbox" />
        Provide <code>#header</code> slot
      </label>
      <label class="toggle">
        <input v-model="includeFooter" type="checkbox" />
        Provide <code>#footer</code> slot
      </label>
      <label class="field">
        <span>id attr</span>
        <input v-model="parentId" type="text" />
      </label>
      <label class="field">
        <span>title attr</span>
        <input v-model="parentTitle" type="text" />
      </label>
      <label class="field">
        <span>data-tag attr</span>
        <input v-model="parentDataTag" type="text" />
      </label>
    </div>

    <SlotsAttrsChild class="parent-class" v-bind="passthroughAttrs">
      <template v-if="includeHeader" #header>
        <strong>Header slot content</strong> (from parent)
      </template>

      <p>
        Default slot content from parent. This is what you see when you use a component like:
        <code>&lt;SlotsAttrsChild&gt;...&lt;/SlotsAttrsChild&gt;</code>.
      </p>

      <template v-if="includeFooter" #footer>
        <em>Footer slot content</em> (from parent)
      </template>
    </SlotsAttrsChild>

    <div class="note">
      <strong>Key point:</strong> the child uses <code>defineOptions({ inheritAttrs: false })</code> and then
      forwards attrs explicitly with <code>v-bind="attrs"</code>.
    </div>
  </div>
</template>

<style scoped>
.demo {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 10px 12px;
  background: #fff3e0;
  border-left: 4px solid #ff9800;
  padding: 14px 16px;
  border-radius: 10px;
}

.toggle {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #bf360c;
  font-weight: 600;
  font-size: 13px;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 6px;
  color: #bf360c;
  font-weight: 600;
  font-size: 12px;
}

.field input {
  padding: 10px 12px;
  border: 2px solid #ffcc80;
  border-radius: 8px;
  background: white;
  font-size: 13px;
}

.field input:focus {
  outline: none;
  border-color: #ff9800;
}

.note {
  background: #e3f2fd;
  border-left: 4px solid #2196f3;
  padding: 14px 16px;
  border-radius: 10px;
  color: #1565c0;
  font-size: 14px;
}
</style>

