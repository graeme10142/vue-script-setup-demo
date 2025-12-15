<script setup>
/**
 * DirectivesDemo.vue
 * Demonstrates: Custom directives in script setup
 * Variables starting with 'v' + capital letter become directives
 */
import { ref } from 'vue'

// Custom directive: v-focus (auto-focus on mount)
const vFocus = {
  mounted: (el) => {
    el.focus()
  }
}

// Custom directive: v-highlight (highlight on hover)
const vHighlight = {
  mounted: (el, binding) => {
    const color = binding.value || '#fff3e0'
    el.style.transition = 'background-color 0.2s'

    el.addEventListener('mouseenter', () => {
      el.style.backgroundColor = color
    })

    el.addEventListener('mouseleave', () => {
      el.style.backgroundColor = ''
    })
  }
}

// Custom directive: v-click-outside
const vClickOutside = {
  mounted: (el, binding) => {
    el._clickOutside = (event) => {
      if (!(el === event.target || el.contains(event.target))) {
        binding.value(event)
      }
    }
    document.addEventListener('click', el._clickOutside)
  },
  unmounted: (el) => {
    document.removeEventListener('click', el._clickOutside)
  }
}

// Demo state
const inputValue = ref('')
const dropdownOpen = ref(false)
const clickOutsideCount = ref(0)

const closeDropdown = () => {
  if (dropdownOpen.value) {
    dropdownOpen.value = false
    clickOutsideCount.value++
  }
}
</script>

<template>
  <div class="directives-demo">
    <!-- v-focus demo -->
    <div class="demo-card">
      <h4>v-focus</h4>
      <p>This input auto-focuses when mounted:</p>
      <input v-focus v-model="inputValue" type="text" placeholder="I'm auto-focused!" />
      <div class="code-snippet">
        <pre>const vFocus = {
  mounted: (el) => el.focus()
}</pre>
      </div>
    </div>

    <!-- v-highlight demo -->
    <div class="demo-card">
      <h4>v-highlight</h4>
      <p>Hover over these elements:</p>
      <div class="highlight-boxes">
        <div v-highlight class="box">Default</div>
        <div v-highlight="'#e8f5e9'" class="box">Green</div>
        <div v-highlight="'#e3f2fd'" class="box">Blue</div>
        <div v-highlight="'#fce4ec'" class="box">Pink</div>
      </div>
      <div class="code-snippet">
        <pre>const vHighlight = {
  mounted: (el, binding) => {
    const color = binding.value || '#fff3e0'
    // Add hover listeners...
  }
}</pre>
      </div>
    </div>

    <!-- v-click-outside demo -->
    <div class="demo-card">
      <h4>v-click-outside</h4>
      <p>Click outside the dropdown to close it:</p>
      <div class="dropdown-container">
        <button @click="dropdownOpen = !dropdownOpen" class="dropdown-btn">
          {{ dropdownOpen ? 'Close' : 'Open' }} Dropdown
        </button>
        <div v-if="dropdownOpen" v-click-outside="closeDropdown" class="dropdown-menu">
          <div class="dropdown-item">Option 1</div>
          <div class="dropdown-item">Option 2</div>
          <div class="dropdown-item">Option 3</div>
        </div>
      </div>
      <p class="counter">Closed by clicking outside: {{ clickOutsideCount }} times</p>
      <div class="code-snippet">
        <pre>&lt;div v-click-outside="closeDropdown"&gt;
  ...
&lt;/div&gt;</pre>
      </div>
    </div>

    <div class="note">
      <strong>How it works:</strong> In script setup, any variable starting with <code>v</code> followed by a capital
      letter (like <code>vFocus</code>, <code>vHighlight</code>) automatically becomes a directive usable as
      <code>v-focus</code>, <code>v-highlight</code>, etc.
    </div>
  </div>
</template>

<style scoped>
.directives-demo {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.demo-card {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 12px;
}

.demo-card h4 {
  color: #42b883;
  margin-bottom: 8px;
  font-family: monospace;
}

.demo-card > p {
  color: #666;
  font-size: 14px;
  margin-bottom: 12px;
}

.demo-card input {
  width: 100%;
  padding: 12px 16px;
  border: 2px solid #42b883;
  border-radius: 8px;
  font-size: 16px;
  margin-bottom: 12px;
}

.demo-card input:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.2);
}

.highlight-boxes {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
}

.box {
  padding: 16px 24px;
  background: white;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 500;
  text-align: center;
}

.dropdown-container {
  position: relative;
  display: inline-block;
  margin-bottom: 12px;
}

.dropdown-btn {
  padding: 12px 24px;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 15px;
  cursor: pointer;
  transition: background 0.2s;
}

.dropdown-btn:hover {
  background: #3aa876;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  margin-top: 8px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  min-width: 180px;
  z-index: 100;
  overflow: hidden;
}

.dropdown-item {
  padding: 12px 16px;
  cursor: pointer;
  transition: background 0.15s;
}

.dropdown-item:hover {
  background: #f5f5f5;
}

.counter {
  font-size: 13px;
  color: #666;
  margin-bottom: 12px;
}

.code-snippet {
  background: #2d2d2d;
  border-radius: 6px;
  overflow: hidden;
}

.code-snippet pre {
  margin: 0;
  padding: 12px 16px;
  font-size: 12px;
  line-height: 1.5;
  color: #f8f8f2;
  overflow-x: auto;
}

.note {
  background: #e3f2fd;
  padding: 16px 20px;
  border-radius: 8px;
  border-left: 4px solid #2196f3;
  font-size: 14px;
  color: #1565c0;
}

.note code {
  background: rgba(0, 0, 0, 0.1);
  padding: 2px 6px;
  border-radius: 4px;
  font-size: 13px;
}
</style>

