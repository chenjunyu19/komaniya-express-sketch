<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps(['fileInfo'])

const percent = ref()
const color = ref()
const icon = ref()

switch (props.fileInfo.status) {
  case 'DOWNLOADING':
    if ('progress' in props.fileInfo) {
      percent.value = props.fileInfo.progress * 100
    } else {
      percent.value = -1
    }
    color.value = 'primary'
    icon.value = 'mdi-download'
    break;
  case 'READY':
    percent.value = 100
    color.value = 'success'
    icon.value = 'mdi-check'
    break;
  case 'ERROR':
    color.value = 'error'
    icon.value = 'mdi-alert'
    break;
  default:
    icon.value = 'mdi-help'
    break;
}
</script>

<template>
  <v-progress-circular :model-value="percent" :indeterminate="percent === -1" :color="color">
    <v-icon :icon="icon" size="x-small"></v-icon>
  </v-progress-circular>
</template>
