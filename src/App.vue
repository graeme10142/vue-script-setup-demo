<script setup>
import { ref } from 'vue'

// Import components - they're automatically available in template!
import BasicCounter from './components/BasicCounter.vue'
import PropsDemo from './components/PropsDemo.vue'
import EmitsDemo from './components/EmitsDemo.vue'
import ModelDemo from './components/ModelDemo.vue'
import ReactivityDemo from './components/ReactivityDemo.vue'
import DirectivesDemo from './components/DirectivesDemo.vue'
import SlotsAttrsDemo from './components/SlotsAttrsDemo.vue'
import ExposeDemo from './components/ExposeDemo.vue'
import SuspenseDemo from './components/SuspenseDemo.vue'
import DualScriptDemo from './components/DualScriptDemo.vue'
import CloudinaryDemo from './components/CloudinaryDemo.vue'
import UploadDemo from './components/UploadDemo.vue'

// Track which demo is active
const activeDemo = ref('counter')
const showDualScriptDemo = ref(true)

const demos = [
  { id: 'counter', name: 'Basic Counter', desc: 'Script setup basics' },
  { id: 'props', name: 'Props Demo', desc: 'defineProps with TypeScript' },
  { id: 'emits', name: 'Emits Demo', desc: 'defineEmits for events' },
  { id: 'model', name: 'v-model Demo', desc: 'defineModel (Vue 3.4+)' },
  { id: 'reactivity', name: 'Reactivity', desc: 'ref, reactive, computed, watch' },
  { id: 'directives', name: 'Directives', desc: 'Custom directives' },
  { id: 'slots-attrs', name: 'Slots & Attrs', desc: 'useSlots + useAttrs' },
  { id: 'expose', name: 'Expose', desc: 'defineExpose + refs' },
  { id: 'suspense', name: 'Suspense', desc: 'top-level await' },
  { id: 'dual-script', name: 'Two Scripts', desc: 'module vs instance' },
  { id: 'cloudinary', name: 'Cloudinary Images', desc: 'Image transformations' },
  { id: 'upload', name: 'Upload Widget', desc: 'Cloudinary uploads' }
]
</script>

<template>
  <div class="app">
    <header class="header">
      <h1>Vue Script Setup Demo</h1>
      <p>Interactive examples from the Cloudinary Blog article</p>
    </header>

    <nav class="nav">
      <button
        v-for="demo in demos"
        :key="demo.id"
        :class="['nav-btn', { active: activeDemo === demo.id }]"
        @click="activeDemo = demo.id"
      >
        <strong>{{ demo.name }}</strong>
        <small>{{ demo.desc }}</small>
      </button>
    </nav>

    <main class="main">
      <!-- Basic Counter Demo -->
      <section v-if="activeDemo === 'counter'" class="demo-section">
        <h2>Basic Script Setup</h2>
        <p>No return statement needed - everything is automatically available in the template.</p>
        <BasicCounter />
      </section>

      <!-- Props Demo -->
      <section v-if="activeDemo === 'props'" class="demo-section">
        <h2>defineProps</h2>
        <p>Type-safe props with TypeScript support. As of Vue 3.5, destructuring is reactive!</p>
        <PropsDemo 
          title="Hello from Parent!" 
          :count="42" 
          :items="['Vue', 'Script', 'Setup']"
        />
      </section>

      <!-- Emits Demo -->
      <section v-if="activeDemo === 'emits'" class="demo-section">
        <h2>defineEmits</h2>
        <p>Type-safe event emission with payload validation.</p>
        <EmitsDemo @notify="(msg) => alert(msg)" />
      </section>

      <!-- Model Demo -->
      <section v-if="activeDemo === 'model'" class="demo-section">
        <h2>defineModel (Vue 3.4+)</h2>
        <p>Two-way binding made simple - no more manual prop + emit boilerplate!</p>
        <ModelDemo />
      </section>

      <!-- Reactivity Demo -->
      <section v-if="activeDemo === 'reactivity'" class="demo-section">
        <h2>Reactivity System</h2>
        <p>ref, reactive, computed, and watch working together.</p>
        <ReactivityDemo />
      </section>

      <!-- Directives Demo -->
      <section v-if="activeDemo === 'directives'" class="demo-section">
        <h2>Custom Directives</h2>
        <p>Variables starting with 'v' become directives automatically.</p>
        <DirectivesDemo />
      </section>

      <!-- Slots + Attrs Demo -->
      <section v-if="activeDemo === 'slots-attrs'" class="demo-section">
        <h2>useSlots + useAttrs</h2>
        <p>Inspect slots and attributes in a child component, with controlled attribute forwarding.</p>
        <SlotsAttrsDemo />
      </section>

      <!-- defineExpose Demo -->
      <section v-if="activeDemo === 'expose'" class="demo-section">
        <h2>defineExpose</h2>
        <p>Expose child refs/methods to the parent via template refs.</p>
        <ExposeDemo />
      </section>

      <!-- Suspense Demo -->
      <section v-if="activeDemo === 'suspense'" class="demo-section">
        <h2>Top-level await + Suspense</h2>
        <p>Top-level await in &lt;script setup&gt; creates an async component; render it under &lt;Suspense&gt;.</p>
        <SuspenseDemo />
      </section>

      <!-- Dual Script Demo -->
      <section v-if="activeDemo === 'dual-script'" class="demo-section">
        <h2>Regular &lt;script&gt; + &lt;script setup&gt;</h2>
        <p>Module-scoped state runs once; script setup runs per component instance.</p>
        <label class="mount-toggle">
          <input v-model="showDualScriptDemo" type="checkbox" />
          Mount DualScriptDemo
        </label>
        <DualScriptDemo v-if="showDualScriptDemo" />
      </section>

      <!-- Cloudinary Demo -->
      <section v-if="activeDemo === 'cloudinary'" class="demo-section">
        <h2>Cloudinary Image Transformations</h2>
        <p>Using @cloudinary/vue with script setup for optimized images.</p>
        <CloudinaryDemo />
      </section>

      <!-- Upload Demo -->
      <section v-if="activeDemo === 'upload'" class="demo-section">
        <h2>Cloudinary Upload Widget</h2>
        <p>Direct uploads using the Cloudinary widget.</p>
        <UploadDemo />
      </section>
    </main>

    <footer class="footer">
      <p>
        Built for the Cloudinary Blog article: 
        <strong>Vue Script Setup: Simple Guide for Vue 3 SFCs</strong>
      </p>
    </footer>
  </div>
</template>

<style>
.app {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
}

.header {
  text-align: center;
  padding: 30px 0;
  background: linear-gradient(135deg, #42b883 0%, #35495e 100%);
  color: white;
  border-radius: 12px;
  margin-bottom: 20px;
}

.header h1 {
  margin-bottom: 8px;
}

.header p {
  opacity: 0.9;
}

.nav {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 10px;
  margin-bottom: 20px;
}

.nav-btn {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
  text-align: left;
}

.nav-btn:hover {
  border-color: #42b883;
}

.nav-btn.active {
  border-color: #42b883;
  background: #e8f5e9;
}

.nav-btn strong {
  color: #333;
  font-size: 14px;
}

.nav-btn small {
  color: #666;
  font-size: 11px;
  margin-top: 4px;
}

.main {
  background: white;
  border-radius: 12px;
  padding: 30px;
  min-height: 400px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.demo-section h2 {
  color: #35495e;
  margin-bottom: 10px;
}

.demo-section > p {
  color: #666;
  margin-bottom: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid #eee;
}

.mount-toggle {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 14px;
  color: #455a64;
  font-size: 14px;
  font-weight: 600;
}

.footer {
  text-align: center;
  padding: 20px;
  color: #666;
  font-size: 14px;
}
</style>
