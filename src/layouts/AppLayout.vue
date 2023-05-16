<script setup lang="ts">
  import LayoutDefault from '@/layouts/LayoutDefault.vue'
  import { markRaw, ref, watch } from 'vue'
  import { useRoute } from 'vue-router'

  const layout = ref()
  const route = useRoute()

  watch(
    () => route.meta?.layout as string | undefined,
    async (metaLayout) => {
      try {
        const component = metaLayout && (await import(`./${metaLayout}.vue`))
        layout.value = markRaw(component?.default || LayoutDefault)
      } catch (e) {
        layout.value = markRaw(LayoutDefault)
      }
    },
    { immediate: true }
  )
</script>

<template>
  <component :is="layout"> <router-view /> </component>
</template>
