<script setup lang="ts">
import { FolderOpened, TakeawayBox } from '@element-plus/icons-vue';
import { computed } from 'vue';

const props = defineProps(['providerInfo'])

const filters = computed(() => [...props.providerInfo.gameFilter, ...props.providerInfo.langFilter])

const _types = ['', 'success', 'info', 'warning', 'danger'] as const

function getElTagTypeById(id: number) {
  return _types[id % _types.length]
}

function rollElTagType() {
  return _types[Math.floor(Math.random() * _types.length)]
}
</script>

<template>
  <el-card class="box-card">
    <template #header>
      <div class="card-header">
        <el-space>
          <el-icon>
            <FolderOpened />
          </el-icon>
          <span>{{ providerInfo.displayName }}</span>
          <el-space>
            <el-tag v-for="filter of filters" :type="rollElTagType()">{{ filter }}</el-tag>
          </el-space>
        </el-space>
        <span>更新时间：{{ new Date(providerInfo.updatedAt).toLocaleString() }}</span>
      </div>
    </template>
    <el-space fill w="full">
      <a v-for="file of providerInfo.files" :href="file.path" class="text">
        <el-card body-class="flex-center" shadow="never">
          <el-icon size="2em" mr="1em">
            <TakeawayBox />
          </el-icon>
          <el-space direction="vertical" alignment="flex-start">
            <div class="file-name">
              {{ file.name }}
            </div>
            <el-space>
              <span>包大小：{{ file.package_size }}</span>
              <span>MD5：{{ file.md5 }}</span>
            </el-space>
            <el-space>
              <el-tag v-for="tag of file.tags" :type="rollElTagType()">{{ tag }}</el-tag>
            </el-space>
          </el-space>
          <download-progress :file-info="file" :width="64" ml="auto" />
        </el-card>
      </a>
    </el-space>
  </el-card>
</template>
<style>
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.flex-center {
  display: flex;
  align-items: center;
}

.text {
  font-size: 14px;
  text-decoration: none;
}

.file-name {
  font-size: 1.25em;
  font-family: monospace, sans-serif;
}
</style>
  