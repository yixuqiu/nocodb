<script setup lang="ts">
import { message } from 'ant-design-vue'

definePageMeta({
  public: true,
  requiresAuth: false,
  layout: 'shared-view',
})

const route = useRoute()

const { loadSharedView } = useSharedView()

const showPassword = ref(false)

try {
  await loadSharedView(route.params.viewId as string)
} catch (e: any) {
  if (e?.response?.status === 403) {
    showPassword.value = true
  } else {
    message.error(await extractSdkResponseErrorMsg(e))
  }
}
</script>

<template>
  <div v-if="showPassword">
    <LazySharedViewAskPassword v-model="showPassword" />
  </div>
  <LazySharedViewMap v-else />
</template>
