<script setup>
// UploadDemo.vue
// Demonstrates: Cloudinary Upload Widget with script setup
// Note: The widget script must be loaded in index.html
// See README for the required script tag

import { ref, computed } from 'vue'

// Upload state
const uploadedImages = ref([])
const isWidgetOpen = ref(false)
const error = ref('')

// For demo purposes - in production, use your own cloud name and preset
// You can get these from your Cloudinary dashboard
const cloudName = ref('demo') // Replace with your cloud name
const uploadPreset = ref('docs_upload_example_us_preset') // Replace with your unsigned preset

const hasCredentials = computed(() => {
  return cloudName.value && uploadPreset.value
})

const openWidget = () => {
  error.value = ''

  // Check if Cloudinary widget is loaded
  if (!window.cloudinary) {
    error.value = 'Cloudinary widget not loaded. Make sure the script is in index.html'
    return
  }

  if (!hasCredentials.value) {
    error.value = 'Please enter your Cloudinary cloud name and upload preset'
    return
  }

  isWidgetOpen.value = true

  const widget = window.cloudinary.createUploadWidget(
    {
      cloudName: cloudName.value,
      uploadPreset: uploadPreset.value,
      sources: ['local', 'url', 'camera'],
      multiple: true,
      maxFiles: 5,
      showAdvancedOptions: false,
      cropping: false,
      styles: {
        palette: {
          window: '#ffffff',
          sourceBg: '#f8f9fa',
          windowBorder: '#90a4ae',
          tabIcon: '#42b883',
          inactiveTabIcon: '#69778c',
          menuIcons: '#42b883',
          link: '#42b883',
          action: '#42b883',
          inProgress: '#42b883',
          complete: '#42b883',
          error: '#c62828',
          textDark: '#263238',
          textLight: '#ffffff'
        }
      }
    },
    (err, result) => {
      if (err) {
        error.value = err.message || 'Upload failed'
        isWidgetOpen.value = false
        return
      }

      if (result.event === 'success') {
        uploadedImages.value.unshift({
          publicId: result.info.public_id,
          url: result.info.secure_url,
          thumbnail: result.info.thumbnail_url,
          format: result.info.format,
          width: result.info.width,
          height: result.info.height,
          bytes: result.info.bytes
        })
      }

      if (result.event === 'close') {
        isWidgetOpen.value = false
      }
    }
  )

  widget.open()
}

const clearImages = () => {
  uploadedImages.value = []
}

const formatBytes = (bytes) => {
  if (bytes < 1024) return bytes + ' B'
  if (bytes < 1024 * 1024) return (bytes / 1024).toFixed(1) + ' KB'
  return (bytes / (1024 * 1024)).toFixed(1) + ' MB'
}
</script>

<template>
  <div class="upload-demo">
    <!-- Configuration -->
    <div class="config-section">
      <h4>Cloudinary Configuration</h4>
      <p class="config-note">
        Using Cloudinary's demo account. For your own uploads,
        <a href="https://cloudinary.com/users/register_free" target="_blank">create a free account</a>
        and get your cloud name + unsigned upload preset.
      </p>
      <div class="config-inputs">
        <div class="input-group">
          <label>Cloud Name</label>
          <input v-model="cloudName" type="text" placeholder="your-cloud-name" />
        </div>
        <div class="input-group">
          <label>Upload Preset</label>
          <input v-model="uploadPreset" type="text" placeholder="your-unsigned-preset" />
        </div>
      </div>
    </div>

    <!-- Upload button -->
    <div class="upload-section">
      <button @click="openWidget" class="upload-btn" :disabled="!hasCredentials || isWidgetOpen">
        <span class="icon">Upload</span>
        {{ isWidgetOpen ? 'Widget Open...' : 'Open Upload Widget' }}
      </button>

      <p v-if="error" class="error">{{ error }}</p>
    </div>

    <!-- Uploaded images -->
    <div v-if="uploadedImages.length > 0" class="results-section">
      <div class="results-header">
        <h4>Uploaded Images ({{ uploadedImages.length }})</h4>
        <button @click="clearImages" class="clear-btn">Clear All</button>
      </div>

      <div class="image-grid">
        <div v-for="img in uploadedImages" :key="img.publicId" class="image-card">
          <img :src="img.thumbnail || img.url" :alt="img.publicId" />
          <div class="image-info">
            <p class="public-id">{{ img.publicId }}</p>
            <p class="meta">{{ img.width }}x{{ img.height }} - {{ img.format.toUpperCase() }} - {{ formatBytes(img.bytes) }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Code example -->
    <div class="code-section">
      <h4>Implementation</h4>
      <div class="code-block">
        <pre>&lt;!-- 1. Add widget script to index.html --&gt;
&lt;script src="https://widget.cloudinary.com/v2.0/global/all.js"&gt;&lt;/script&gt;

&lt;!-- 2. Use in your component --&gt;
&lt;script setup&gt;
import { ref } from 'vue'

const uploadedUrl = ref('')

const openWidget = () => {
  window.cloudinary.openUploadWidget(
    {
      cloudName: 'your-cloud-name',
      uploadPreset: 'your-unsigned-preset'
    },
    (error, result) => {
      if (result?.event === 'success') {
        uploadedUrl.value = result.info.secure_url
      }
    }
  )
}
&lt;/script&gt;</pre>
      </div>
    </div>
  </div>
</template>

<style scoped>
.upload-demo {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.config-section {
  background: #fff3e0;
  padding: 20px;
  border-radius: 12px;
  border-left: 4px solid #ff9800;
}

.config-section h4 {
  color: #e65100;
  margin-bottom: 8px;
}

.config-note {
  font-size: 14px;
  color: #bf360c;
  margin-bottom: 16px;
}

.config-note a {
  color: #e65100;
  font-weight: 600;
}

.config-inputs {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

@media (max-width: 500px) {
  .config-inputs {
    grid-template-columns: 1fr;
  }
}

.input-group label {
  display: block;
  font-size: 12px;
  font-weight: 600;
  color: #bf360c;
  margin-bottom: 6px;
}

.input-group input {
  width: 100%;
  padding: 10px 12px;
  border: 2px solid #ffcc80;
  border-radius: 6px;
  font-size: 14px;
  background: white;
}

.input-group input:focus {
  outline: none;
  border-color: #ff9800;
}

.upload-section {
  text-align: center;
}

.upload-btn {
  padding: 16px 32px;
  background: linear-gradient(135deg, #42b883 0%, #35495e 100%);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  display: inline-flex;
  align-items: center;
  gap: 10px;
}

.upload-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(66, 184, 131, 0.3);
}

.upload-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.icon {
  font-size: 16px;
  font-weight: 900;
}

.error {
  color: #c62828;
  margin-top: 12px;
  font-size: 14px;
}

.results-section {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 12px;
}

.results-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.results-header h4 {
  color: #35495e;
}

.clear-btn {
  padding: 6px 12px;
  background: #ef5350;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 13px;
  cursor: pointer;
}

.clear-btn:hover {
  background: #e53935;
}

.image-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 16px;
}

.image-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.image-card img {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.image-info {
  padding: 12px;
}

.public-id {
  font-size: 12px;
  font-weight: 600;
  color: #35495e;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-bottom: 4px;
}

.meta {
  font-size: 11px;
  color: #999;
}

.code-section h4 {
  color: #35495e;
  margin-bottom: 12px;
}

.code-block {
  background: #2d2d2d;
  border-radius: 8px;
  overflow: hidden;
}

.code-block pre {
  margin: 0;
  padding: 16px;
  font-size: 12px;
  line-height: 1.5;
  color: #f8f8f2;
  overflow-x: auto;
}
</style>

