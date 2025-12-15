# **Vue Script Setup: Simple Guide for Vue 3 SFCs**

Writing Vue components used to mean writing an awful lot of repetitive code. We'd define our data and write our methods, then remember to return everything from the setup function \- all so that the template could have access to it. Thankfully, Vue 3's `<script setup>` syntax changes this by giving us a much cleaner way to write Single File Components (SFCs).

Script setup means that we need less boilerplate and get better TypeScript support along with better runtime performance. We'll explore this syntax and how it works and cover the basic patterns that we need for props and reactivity. We’ll also see how [Cloudinary's Vue SDK](https://cloudinary.com/documentation/vue_integration) fits into our modern Vue 3 workflows.

### **Key Takeaways**

* Script setup eliminates boilerplate code so we don’t need manual return statements  
* Components and composables become available in templates automatically after importing  
* defineProps, defineEmits, and defineModel (Vue 3.4+) simplify component communication  
* Cloudinary's Vue SDK works with script setup for smooth and optimized image delivery

## **What Is Script Setup and Why Use It?**

The `<script setup>` syntax is compile-time syntactic sugar for using the Composition API inside Vue Single File Components. Instead of wrapping our code in a setup function and returning everything back manually, we write our logic directly at the top level of the script block.

If you’re curious about why this is a big deal then here's what makes it worth using. First, we write much less code because we don’t need export default or return statements. Second, the Vue compiler can optimize our templates better because it knows exactly what variables are available for it to use. Third, we get better IDE support and type inference, making our development tasks faster while it also catches errors earlier.

Even better, the performance benefits are a real improvement. Script setup compiles directly into a render function without us needing to provide it with an intermediate proxy. What we get is faster component initialization and better runtime performance within our application.

## **Syntax Basics**

Getting started with script setup is not difficult either. All we add is the `setup` attribute to our script tag, and everything inside becomes available to our template automatically.

Let's compare the old way with the new way. Here's a component using the traditional Composition API:

| \<script\>import { ref, computed } from 'vue'export default {  setup() {    const count \= ref(0)    const doubled \= computed(() \=\> count.value \* 2)        const increment \= () \=\> {      count.value++    }        return { count, doubled, increment }  }}\</script\> |
| :---- |

Now here's the same component with script setup:

| \<script setup\>import { ref, computed } from 'vue'const count \= ref(0)const doubled \= computed(() \=\> count.value \* 2)const increment \= () \=\> {  count.value++}\</script\> |
| :---- |

We've cut out the boilerplate entirely\! We have gotten rid of `export default`, the `setup()` function wrapper, and we have no return statement. Every top-level variable, function, and import is now automatically available in our template.

And there’s more. Importing components works the same way \- we just import them and use them directly \- so don’t need to register them in a components option:

| \<script setup\>import MyButton from './MyButton.vue'import UserCard from './UserCard.vue'\</script\>\<template\>  \<UserCard\>    \<MyButton\>Click me\</MyButton\>  \</UserCard\>\</template\> |
| :---- |

## **Data Flow: Props, Emits, and v-model**

There are a few things to keep in mind though. Since we don't have a traditional options object anymore we’ll need a different way to define props and events. Vue gives us compiler macros that handle this for us: `defineProps`, `defineEmits`, and `defineModel`.

These macros are automatically available inside `<script setup>` so we don't need to import them. Here's how we define props:

| \<script setup\>const props \= defineProps({  title: String,  count: {    type: Number,    default: 0  }})console.log(props.title)\</script\> |
| :---- |

If we're using TypeScript, we can use type-based declarations instead; this gives us better type inference and it also feels more natural:

| \<script setup lang="ts"\>interface Props {  title: string  count?: number}const props \= defineProps\<Props\>()\</script\> |
| :---- |

For default values with TypeScript, we use the `withDefaults` helper:

| \<script setup lang="ts"\>interface Props {  title: string  count?: number  items?: string\[\]}const props \= withDefaults(defineProps\<Props\>(), {  count: 0,  items: () \=\> \[\]})\</script\> |
| :---- |

**Pro tip:** Starting at Vue 3.5, we can safely destructure props and they still stay reactive. This means that const { title, count } \= defineProps\<Props\>() works without breaking reactivity \- something that wasn't possible in earlier versions.

Emitting events follows a similar pattern with `defineEmits`:

| \<script setup\>const emit \= defineEmits(\['update', 'delete', 'select'\])const handleClick \= () \=\> {  emit('update', { id: 1, value: 'new value' })}\</script\> |
| :---- |

With TypeScript, we can add type safety to our events:

| \<script setup lang="ts"\>const emit \= defineEmits\<{  (e: 'update', payload: { id: number; value: string }): void  (e: 'delete', id: number): void}\>()\</script\> |
| :---- |

### **Two-Way Binding With defineModel**

Vue 3.4 gives us `defineModel`, which gives us simplified two-way binding. Before this, implementing `v-model` on a custom component meant that we had to define a `modelValue` prop and manually emit an `update:modelValue`. Now we can do it in one line.

Here's the old way compared to the new way:

| \<script setup\>// OLD WAY (Vue 3.3 and earlier)// const props \= defineProps(\['modelValue'\])// const emit \= defineEmits(\['update:modelValue'\])// const updateValue \= (val) \=\> emit('update:modelValue', val)// NEW WAY (Vue 3.4+)const model \= defineModel()function increment() {  model.value++ // Automatically emits 'update:modelValue'}\</script\>\<template\>  \<button @click="increment"\>Count: {{ model }}\</button\>\</template\> |
| :---- |

The parent component uses it like any other `v-model`:

| \<template\>  \<CounterButton v-model="count" /\>\</template\> |
| :---- |

We can also define more than one model with different names and add type safety:

| \<script setup lang="ts"\>const firstName \= defineModel\<string\>('firstName')const lastName \= defineModel\<string\>('lastName')\</script\> |
| :---- |

You could argue that this is one of the biggest quality-of-life improvements in recent Vue releases because it saves space and time.

## **Reactivity: ref, reactive, computed, and watch**

Vue's reactivity system works exactly the same way in script setup; we import `ref`, `reactive`, `computed`, and `watch` from Vue and use them at the top level.

Here's a good example showing common reactivity patterns:

| \<script setup\>import { ref, reactive, computed, watch } from 'vue'// Simple reactive valueconst searchQuery \= ref('')// Reactive object for form dataconst formData \= reactive({  name: '',  email: '',  message: ''})// Computed property that updates automaticallyconst isFormValid \= computed(() \=\> {  return formData.name.length \> 0 &&          formData.email.includes('@') &&          formData.message.length \> 10})// Watch for changeswatch(searchQuery, (newValue, oldValue) \=\> {  console.log(\`Search changed from "${oldValue}" to "${newValue}"\`)})\</script\>\<template\>  \<input v-model="searchQuery" placeholder="Search..."\>    \<form\>    \<input v-model="formData.name" placeholder="Name"\>    \<input v-model="formData.email" placeholder="Email"\>    \<textarea v-model="formData.message"\>\</textarea\>        \<button :disabled="\!isFormValid"\>Submit\</button\>  \</form\>\</template\> |
| :---- |

The `watchEffect` function is also there for when we want to automatically track reactive dependencies:

| \<script setup\>import { ref, watchEffect } from 'vue'const userId \= ref(1)const userData \= ref(null)watchEffect(async () \=\> {  const response \= await fetch(\`/api/users/${userId.value}\`)  userData.value \= await response.json()})\</script\> |
| :---- |

This code automatically re-runs whenever `userId` changes, keeping our data in sync without manually specifying dependencies.

## **Components, Slots, and Directives**

The components we import are available right away in our templates. This includes local components and also from libraries. Vue handles the registration automatically during compilation.

For accessing slots and attributes we use the `useSlots` and `useAttrs` composables:

| \<script setup\>import { useSlots, useAttrs } from 'vue'const slots \= useSlots()const attrs \= useAttrs()// Check if a slot was providedconst hasHeader \= computed(() \=\> \!\!slots.header)\</script\> |
| :---- |

We also get custom directives that follow a naming convention in script setup. Any variable starting with `v` followed by a capital letter is available as a directive:

| \<script setup\>const vFocus \= {  mounted: (el) \=\> el.focus()}const vHighlight \= {  mounted: (el, binding) \=\> {    el.style.backgroundColor \= binding.value || 'yellow'  }}\</script\>\<template\>  \<input v-focus\>  \<p v-highlight="'lightblue'"\>This text is highlighted\</p\>\</template\> |
| :---- |

When we want to expose properties to parent components (like for template refs), we’ll use `defineExpose`:

| \<script setup\>import { ref } from 'vue'const count \= ref(0)const publicMethod \= () \=\> {  console.log('Called from parent\!')}// Only these will be accessible via template refdefineExpose({  count,  publicMethod})\</script\> |
| :---- |

Script setup components are "closed" by default which means that parent components can't access anything inside them through template refs unless we explicitly expose it.

## **TypeScript Tips, Gotchas, and Performance**

TypeScript works beautifully with script setup. We add `lang="ts"` to our script tag and get full type inference in our component.

| \<script setup lang="ts"\>import { ref } from 'vue'// Type is inferred as Ref\<number\>const count \= ref(0)// We can also be explicitconst message \= ref\<string | null\>(null)\</script\> |
| :---- |

For generic components, Vue 3.3 introduced the `generic` attribute:

| \<script setup lang="ts" generic="T"\>defineProps\<{  items: T\[\]  selected: T}\>()\</script\> |
| :---- |

We have to be weary though \- here are some common gotchas to watch out for. First, `defineProps` and `defineEmits` are compiler macros, **not** runtime functions. We can't use them conditionally or store them in variables to use later. Second, top-level `await` is supported but it needs wrapping of the parent component in `<Suspense>`. Third, if we need component options like `inheritAttrs: false`, then we can use `defineOptions` (Vue 3.3+) or add a separate regular `<script>` block.

| \<script setup\>defineOptions({  inheritAttrs: false})\</script\> |
| :---- |

Script setup components compile more efficiently, giving us a performance boost. The template compiler can access variables without going through a proxy object, giving us faster component initialization.

## **Migrating From Options or Composition API**

There’s more good news: moving to script setup doesn't have to happen all at once. Instead, we can migrate components gradually and even use both syntaxes in the same component if we need to.

If we have a component using the Composition API with `setup()`, then the migration is mostly smooth sailing. We remove the wrapper function, delete the return statement, and add `setup` to our script tag. Props and emits move to their own respective macros.

For Options API components, we'll need to refactor the logic into Composition API patterns first. This usually means that we need to convert the `data` properties to `ref` or `reactive`, which turns `computed` properties into `computed()` calls, and converts methods into regular functions.

We can use a regular `<script>` block alongside `<script setup>` when we need features that script setup doesn't support directly:

| \<script\>// Runs once per module, not per component instanceexport const sharedState \= reactive({ count: 0 })export default {  name: 'MyComponent',  inheritAttrs: false}\</script\>\<script setup\>// Runs for each component instanceimport { ref } from 'vue'const localCount \= ref(0)\</script\> |
| :---- |

The two script blocks are merged during compilation, giving us the best of both worlds.

## **Working With Cloudinary in Vue 3 Script Setup**

Cloudinary's Vue SDK integrates smoothly with script setup. We can [handle image asset bundling with Vite](https://cloudinary.com/blog/handle-image-asset-bundling-using-vite-in-vuejs) and deliver optimized images with just a few lines of code.

First, we install the necessary packages:

| npm install @cloudinary/vue @cloudinary/url-gen |
| :---- |

Then we create a component using script setup to display and transform images:

| \<script setup\>import { AdvancedImage } from '@cloudinary/vue'import { Cloudinary } from '@cloudinary/url-gen'import { fill } from '@cloudinary/url-gen/actions/resize'import { autoGravity } from '@cloudinary/url-gen/qualifiers/gravity'// Initialize Cloudinary instanceconst cld \= new Cloudinary({  cloud: {    cloudName: 'our-cloud-name'  }})// Create an image with transformationsconst productImage \= cld  .image('products/sample-product')  .resize(fill().width(400).height(400).gravity(autoGravity()))  .format('auto')  .quality('auto')\</script\>\<template\>  \<AdvancedImage :cldImg="productImage" /\>\</template\> |
| :---- |

This code creates a responsive, automatically optimized image that focuses on the most important part of the picture. Cloudinary handles format selection and compression automatically, so our users end up with the best possible experience.

We can also [upload images directly from Vue](https://cloudinary.com/blog/guest_post/how-to-upload-images-in-vue-js-with-drag-n-drop) using the Upload Widget. The widget works great with script setup's clean syntax.

**Note:** The Upload Widget is a separate dependency from the Vue SDK packages. We need to include the widget script in our `index.html` file:

| \<script src="https://widget.cloudinary.com/v2.0/global/all.js" type="text/javascript"\>\</script\> |
| :---- |

Once the script is loaded, we can use it in our component:

| \<script setup\>import { ref } from 'vue'const uploadedUrl \= ref('')const openWidget \= () \=\> {  window.cloudinary.openUploadWidget(    {      cloudName: 'our-cloud-name',      uploadPreset: 'our-preset'    },    (error, result) \=\> {      if (result?.event \=== 'success') {        uploadedUrl.value \= result.info.secure\_url      }    }  )}\</script\>\<template\>  \<button @click="openWidget"\>Upload Image\</button\>  \<img v-if="uploadedUrl" :src="uploadedUrl" alt="Uploaded image"\>\</template\> |
| :---- |

The combination of script setup's simplified boilerplate and Cloudinary's automatic optimization means we can build media capable Vue applications with less effort and minimal code.

## **Final Thoughts**

The bottom line is that script setup gives us a cleaner way to write Vue components. We get less boilerplate, better integration with TypeScript, and impressive performance \- all without us needing to sacrifice any functionality. The syntax feels natural once we understand how the compiler macros work, and migration from existing code can happen gradually over time.

For projects that need optimized media delivery, Cloudinary's Vue SDK fits right into script setup components. We can transform and deliver images efficiently while keeping our code minimal and readable.

If we're ready to optimize our images and videos in Vue, we can [sign up with Cloudinary](https://cloudinary.com/users/register_free) and register for a free account. See how automatic optimization can speed up our Vue applications.

## **FAQs**

### **Can We Use Script Setup With Vue 2?**

Vue 2.7 (the final version of Vue 2\) natively supports `<script setup>` and the Composition API so there are no plugins required. If we're on an older version of Vue 2, we can use the `unplugin-vue2-script-setup` community plugin to add the same functionality. However, if we're starting a new project or have the option to upgrade, Vue 3 is the better choice for full script setup support and future improvements.

### **Do We Need TypeScript for Script Setup?**

No, TypeScript is completely optional, and script setup works perfectly with plain JavaScript. TypeScript adds type safety and better IDE support, but it's not required. We can start with JavaScript and add TypeScript later if our project needs it.

### **What Happens to the Options API?**

The Options API isn't going anywhere. Vue 3 fully supports both the Options API and Composition API, and we can even mix them in the same project. Script setup is just a more concise way to use the Composition API in Single File Components. Teams can choose the approach that fits their preferences and project.

