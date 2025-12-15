# Vue Script Setup Demo

Interactive demo project for the article: **"Vue Script Setup: Simple Guide for Vue 3 SFCs"**

![Vue 3](https://img.shields.io/badge/Vue-3.5-42b883?style=flat-square&logo=vue.js)
![Vite](https://img.shields.io/badge/Vite-6.0-646cff?style=flat-square&logo=vite)
![Cloudinary](https://img.shields.io/badge/Cloudinary-SDK-3448c5?style=flat-square)

## Quick Start

```bash
# Clone the repository
git clone https://github.com/graeme10142/vue-script-setup-demo.git

# Navigate to project
cd vue-script-setup-demo

# Install dependencies
npm install

# Start development server
npm run dev
```

Open `http://localhost:3000` in your browser.

## What's Covered

This demo mirrors the article's key concepts:

| Demo | Concepts | File(s) |
|------|----------|---------|
| **Basic Counter** | `<script setup>` basics, no return statement | `BasicCounter.vue` |
| **Props Demo** | `defineProps`, `withDefaults` | `PropsDemo.vue` |
| **Emits Demo** | `defineEmits` | `EmitsDemo.vue` |
| **v-model Demo** | `defineModel` (Vue 3.4+), custom component v-model | `ModelDemo.vue`, `ModelCounter.vue`, `ModelInput.vue` |
| **Reactivity** | `ref`, `reactive`, `computed`, `watch`, `watchEffect` | `ReactivityDemo.vue` |
| **Directives** | Script-setup directives (`vFocus` -> `v-focus`) | `DirectivesDemo.vue` |
| **Slots & Attrs** | `useSlots`, `useAttrs`, `defineOptions({ inheritAttrs: false })` | `SlotsAttrsDemo.vue`, `SlotsAttrsChild.vue` |
| **Expose** | `defineExpose` + template refs | `ExposeDemo.vue`, `ExposeChild.vue` |
| **Suspense** | Top-level `await` + `<Suspense>` | `SuspenseDemo.vue`, `SuspenseAsyncDemo.vue` |
| **Two Scripts** | `<script>` vs `<script setup>` scope | `DualScriptDemo.vue` |
| **Cloudinary Images** | `@cloudinary/vue`, transformations | `CloudinaryDemo.vue` |
| **Upload Widget** | Cloudinary Upload Widget integration | `UploadDemo.vue` |

## Cloudinary Integration

This demo uses Cloudinary for image transformations and uploads.

### Image Transformations

```vue
<script setup>
import { AdvancedImage } from '@cloudinary/vue'
import { Cloudinary } from '@cloudinary/url-gen'
import { fill } from '@cloudinary/url-gen/actions/resize'

const cld = new Cloudinary({ cloud: { cloudName: 'demo' } })

const img = cld
  .image('sample')
  .resize(fill().width(400).height(400))
  .format('auto')
  .quality('auto')
</script>

<template>
  <AdvancedImage :cldImg="img" />
</template>
```

### Upload Widget

The Upload Widget requires a script tag in `index.html`:

```html
<script src="https://widget.cloudinary.com/v2.0/global/all.js"></script>
```

## Project Structure

```
vue-script-setup-demo/
  index.html
  package.json
  vite.config.js
  src/
    main.js
    App.vue
    components/
      BasicCounter.vue
      PropsDemo.vue
      EmitsDemo.vue
      ModelDemo.vue
      ModelCounter.vue
      ModelInput.vue
      ReactivityDemo.vue
      DirectivesDemo.vue
      SlotsAttrsDemo.vue
      SlotsAttrsChild.vue
      ExposeDemo.vue
      ExposeChild.vue
      SuspenseDemo.vue
      SuspenseAsyncDemo.vue
      DualScriptDemo.vue
      CloudinaryDemo.vue
      UploadDemo.vue
```

## Technologies Used

- **Vue 3.5** - Progressive JavaScript framework
- **Vite 6** - Next-generation frontend tooling
- **@cloudinary/vue** - Cloudinary Vue SDK
- **@cloudinary/url-gen** - URL generation and transformations

## License

MIT License - feel free to use this code for learning and projects.

