<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps(['fileInfo'])

const perc = ref(0)
const stat = ref()

switch (props.fileInfo.status) {
  case 'DOWNLOADING':
    perc.value = Math.round((props.fileInfo.progress ?? 0) * 100)
    break;
  case 'READY':
    perc.value = 100;
    stat.value = 'success'
    break;
  case 'ERROR':
    stat.value = 'exception'
    break;
  default:
    break;
}
</script>

<template>
  <el-progress
    type="circle"
    :percentage="perc"
    :status="stat"
  />
</template>
