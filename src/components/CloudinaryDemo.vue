<script setup>
/**
 * CloudinaryDemo.vue
 * Demonstrates: Cloudinary Vue SDK with script setup
 * Shows image transformations and automatic optimization
 */
import { ref, computed } from 'vue'
import { AdvancedImage } from '@cloudinary/vue'
import { Cloudinary } from '@cloudinary/url-gen'
import { fill, scale, thumbnail } from '@cloudinary/url-gen/actions/resize'
import { autoGravity, focusOn } from '@cloudinary/url-gen/qualifiers/gravity'
import { face } from '@cloudinary/url-gen/qualifiers/focusOn'
import { sepia, grayscale, blur } from '@cloudinary/url-gen/actions/effect'
import { max } from '@cloudinary/url-gen/actions/roundCorners'

// Initialize Cloudinary with demo cloud
const cld = new Cloudinary({
  cloud: {
    cloudName: 'demo' // Using Cloudinary's demo cloud
  }
})

// Demo image public ID (from Cloudinary demo account)
const publicId = 'samples/people/smiling-man'

// Transformation options
const selectedEffect = ref('none')
const selectedCrop = ref('fill')
const imageWidth = ref(300)
const roundCorners = ref(false)

// Build the Cloudinary image with transformations
const cloudinaryImage = computed(() => {
  let img = cld.image(publicId)

  // Apply resize/crop
  switch (selectedCrop.value) {
    case 'fill':
      img = img.resize(fill().width(imageWidth.value).height(imageWidth.value).gravity(autoGravity()))
      break
    case 'scale':
      img = img.resize(scale().width(imageWidth.value))
      break
    case 'thumb':
      img = img.resize(
        thumbnail().width(imageWidth.value).height(imageWidth.value).gravity(focusOn(face()))
      )
      break
  }

  // Apply effects
  switch (selectedEffect.value) {
    case 'sepia':
      img = img.effect(sepia())
      break
    case 'grayscale':
      img = img.effect(grayscale())
      break
    case 'blur':
      img = img.effect(blur().strength(500))
      break
  }

  // Apply round corners
  if (roundCorners.value) {
    img = img.roundCorners(max())
  }

  // Auto format and quality for optimization
  img = img.format('auto').quality('auto')

  return img
})

// Generate the URL for display
const imageUrl = computed(() => cloudinaryImage.value.toURL())
</script>

<template>
  <div class="cloudinary-demo">
    <div class="demo-layout">
      <!-- Image preview -->
      <div class="preview-section">
        <div class="image-container">
          <AdvancedImage :cldImg="cloudinaryImage" alt="Cloudinary transformed image" />
        </div>
        <p class="url-display">
          <strong>Generated URL:</strong><br />
          <code>{{ imageUrl }}</code>
        </p>
      </div>

      <!-- Controls -->
      <div class="controls-section">
        <h4>Transformation Controls</h4>

        <div class="control-group">
          <label>Crop Mode</label>
          <div class="radio-group">
            <label>
              <input type="radio" v-model="selectedCrop" value="fill" />
              Fill (auto gravity)
            </label>
            <label>
              <input type="radio" v-model="selectedCrop" value="scale" />
              Scale (proportional)
            </label>
            <label>
              <input type="radio" v-model="selectedCrop" value="thumb" />
              Thumbnail (face focus)
            </label>
          </div>
        </div>

        <div class="control-group">
          <label>Width: {{ imageWidth }}px</label>
          <input type="range" v-model.number="imageWidth" min="100" max="500" step="50" />
        </div>

        <div class="control-group">
          <label>Effect</label>
          <select v-model="selectedEffect">
            <option value="none">None</option>
            <option value="sepia">Sepia</option>
            <option value="grayscale">Grayscale</option>
            <option value="blur">Blur</option>
          </select>
        </div>

        <div class="control-group">
          <label class="checkbox-label">
            <input type="checkbox" v-model="roundCorners" />
            Round corners (circle)
          </label>
        </div>

        <div class="code-block">
          <pre>// Cloudinary + Script Setup
import { AdvancedImage } from '@cloudinary/vue'
import { Cloudinary } from '@cloudinary/url-gen'

const cld = new Cloudinary({
  cloud: { cloudName: 'demo' }
})

const img = cld.image('samples/people/smiling-man')
  .resize(fill().width(300).gravity(autoGravity()))
  .format('auto')
  .quality('auto')</pre>
        </div>
      </div>
    </div>

    <div class="note">
      <strong>Automatic Optimization:</strong> Notice <code>.format('auto').quality('auto')</code> -> Cloudinary
      automatically delivers the best format (WebP, AVIF, etc.) and compression level based on the user's browser and
      connection.
    </div>
  </div>
</template>

<style scoped>
.cloudinary-demo {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.demo-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
}

@media (max-width: 700px) {
  .demo-layout {
    grid-template-columns: 1fr;
  }
}

.preview-section {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.image-container {
  background: #f0f0f0;
  border-radius: 12px;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 350px;
}

.image-container :deep(img) {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.url-display {
  background: #263238;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 12px;
  color: #90a4ae;
  overflow-x: auto;
}

.url-display strong {
  color: #b0bec5;
}

.url-display code {
  color: #80cbc4;
  word-break: break-all;
}

.controls-section {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 12px;
}

.controls-section h4 {
  color: #35495e;
  margin-bottom: 20px;
  padding-bottom: 12px;
  border-bottom: 2px solid #e0e0e0;
}

.control-group {
  margin-bottom: 20px;
}

.control-group > label {
  display: block;
  font-size: 13px;
  font-weight: 600;
  color: #666;
  margin-bottom: 8px;
}

.radio-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.radio-group label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  cursor: pointer;
}

input[type='range'] {
  width: 100%;
  cursor: pointer;
}

select {
  width: 100%;
  padding: 10px 12px;
  border: 2px solid #e0e0e0;
  border-radius: 6px;
  font-size: 14px;
  background: white;
  cursor: pointer;
}

select:focus {
  outline: none;
  border-color: #42b883;
}

.checkbox-label {
  display: flex !important;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-size: 14px !important;
  font-weight: normal !important;
}

.code-block {
  background: #2d2d2d;
  border-radius: 8px;
  overflow: hidden;
  margin-top: 16px;
}

.code-block pre {
  margin: 0;
  padding: 16px;
  font-size: 11px;
  line-height: 1.5;
  color: #f8f8f2;
  overflow-x: auto;
}

.note {
  background: #e8f5e9;
  padding: 16px 20px;
  border-radius: 8px;
  border-left: 4px solid #42b883;
  font-size: 14px;
  color: #2e7d32;
}

.note code {
  background: rgba(0, 0, 0, 0.1);
  padding: 2px 6px;
  border-radius: 4px;
}
</style>

