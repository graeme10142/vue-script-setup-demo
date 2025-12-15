<script setup>
import SuspenseAsyncDemo from './SuspenseAsyncDemo.vue'
</script>

<template>
  <div class="demo">
    <p class="lead">
      The article notes that top-level <code>await</code> in <code>&lt;script setup&gt;</code> requires wrapping the
      parent in <code>&lt;Suspense&gt;</code>.
    </p>

    <Suspense>
      <template #default>
        <SuspenseAsyncDemo />
      </template>
      <template #fallback>
        <div class="fallback">
          Loading async component...
        </div>
      </template>
    </Suspense>
  </div>
</template>

<style scoped>
.demo {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.lead {
  margin: 0;
  color: #455a64;
  font-size: 14px;
}

.fallback {
  background: #263238;
  color: #eceff1;
  border-radius: 12px;
  padding: 18px;
  font-weight: 700;
}
</style>

