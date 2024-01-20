<script setup lang="ts">
import { ref } from 'vue'

const status = ref([])

async function fetchStatus() {
  const res = await fetch('status.json')
  status.value = await res.json()
}

fetchStatus();
</script>

<template>
  <el-config-provider namespace="ep">
    <BaseHeader class="app-header" />
    <div class="flex main-container">
      <div w="full" p="4">
        <!-- <Logos my="4" />
        <HelloWorld msg="Hello Vue 3 + Element Plus + Vite" /> -->
        <el-space v-if="status?.length" fill>
          <ProviderCard v-for="info of status" :provider-info="info" />
        </el-space>
        <el-empty v-else h="full" description="没有数据" />
      </div>
    </div>
  </el-config-provider>
</template>

<style>
.app-header {
  box-shadow: var(--ep-box-shadow);
}

.main-container {
  height: calc(100vh - var(--ep-menu-item-height) - 4px);
}
</style>
