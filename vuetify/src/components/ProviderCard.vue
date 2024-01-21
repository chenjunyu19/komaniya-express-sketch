<script setup lang="ts">
import { computed, ref } from 'vue';

const props = defineProps(['providerInfo'])

const snackbar = ref(false)

const packages = computed(() => {
  const games = []
  const langs = []
  for (const file of props.providerInfo.files) {
    if (file.tags.some((tag: any) => props.providerInfo.langFilter.includes(tag))) {
      langs.push(file)
    } else {
      games.push(file)
    }
  }
  return { games, langs }
})

function getHref(fileInfo: any) {
  if (fileInfo.status === 'READY') {
    return fileInfo.path
  } else {
    return 'javascript:alert("文件还没准备好")'
  }
}

function formatSize(size: number) {
  const UNITS = ['B', 'KiB', 'MiB', 'GiB']
  for (let i = 0; i < UNITS.length; i++) {
    if (size < 1024) {
      return `${size.toFixed(2)} ${UNITS[i]}`
    } else {
      size /= 1024
    }
  }
}
</script>

<template>
  <v-card :elevation="4">
    <v-card-title class="d-flex align-center flex-wrap ga-2">
      <v-icon start icon="mdi-package-variant"></v-icon>
      <span>{{ providerInfo.displayName }}</span>
      <v-chip v-for="filter of providerInfo.gameFilter" prepend-icon="mdi-filter" label color="primary">
        {{ filter }}
      </v-chip>
      <v-chip v-for="filter of providerInfo.langFilter" prepend-icon="mdi-translate" label color="secondary">
        {{ filter }}
      </v-chip>
      <v-chip class="ms-auto" prepend-icon="mdi-clock" label>
        {{ new Date(providerInfo.updatedAt).toLocaleString() }}
      </v-chip>
    </v-card-title>
    <v-list>
      <v-list-subheader inset>游戏本体</v-list-subheader>
      <a v-for="file of packages.games" class="link" :href="getHref(file)" :download="file.name">
        <v-list-item v-ripple>
          <template v-slot:prepend>
            <v-avatar color="primary">
              <v-icon icon="mdi-zip-box"></v-icon>
            </v-avatar>
          </template>
          <template v-slot:title>
            <div class="d-flex align-center flex-wrap ga-2">
              <code>{{ file.name }}</code>
              <v-chip v-for="tag of file.tags" label
                :color="providerInfo.langFilter.includes(tag) ? 'secondary' : 'primary'" density="compact">
                {{ tag }}
              </v-chip>
            </div>
          </template>
          <template v-slot:subtitle>
            <div class="d-flex align-stretch flex-wrap ga-4">
              <span class="d-flex align-center">
                <v-icon class="mx-1 my-auto" icon="mdi-pound" size="small"></v-icon>
                <code>{{ file.md5 }}</code>
              </span>
              <span class="d-flex align-center">
                <v-icon class="mx-1 my-auto" icon="mdi-package-variant-closed" size="small"></v-icon>
                <span>{{ formatSize(file.package_size) }}</span>
              </span>
              <span class="d-flex align-center">
                <v-icon class="mx-1 my-auto" icon="mdi-harddisk" size="small"></v-icon>
                <span>{{ formatSize(file.size) }}</span>
              </span>
            </div>
          </template>
          <template v-slot:append>
            <DownloadProgress :file-info="file"></DownloadProgress>
          </template>
        </v-list-item>
      </a>

      <v-divider inset></v-divider>

      <v-list-subheader inset>语音包</v-list-subheader>
      <a v-for="file of packages.langs" class="link" :href="getHref(file)" :download="file.name">
        <v-list-item v-ripple>
          <template v-slot:prepend>
            <v-avatar color="secondary">
              <v-icon icon="mdi-translate"></v-icon>
            </v-avatar>
          </template>
          <template v-slot:title>
            <div class="d-flex align-center flex-wrap ga-2">
              <code>{{ file.name }}</code>
              <v-chip v-for="tag of file.tags" label
                :color="providerInfo.langFilter.includes(tag) ? 'secondary' : 'primary'" density="compact">
                {{ tag }}
              </v-chip>
            </div>
          </template>
          <template v-slot:subtitle>
            <div class="d-flex align-stretch flex-wrap ga-4">
              <span class="d-flex align-center">
                <v-icon class="mx-1 my-auto" icon="mdi-pound" size="small"></v-icon>
                <code>{{ file.md5 }}</code>
              </span>
              <span class="d-flex align-center">
                <v-icon class="mx-1 my-auto" icon="mdi-package-variant-closed" size="small"></v-icon>
                <span>{{ formatSize(file.package_size) }}</span>
              </span>
              <span class="d-flex align-center">
                <v-icon class="mx-1 my-auto" icon="mdi-harddisk" size="small"></v-icon>
                <span>{{ formatSize(file.size) }}</span>
              </span>
            </div>
          </template>
          <template v-slot:append>
            <DownloadProgress :file-info="file"></DownloadProgress>
          </template>
        </v-list-item>
      </a>
    </v-list>
  </v-card>
  <v-snackbar v-model="snackbar">文件还没有准备好</v-snackbar>
</template>

<style scoped>
.link {
  text-decoration: unset;
  color: unset;
}
</style>
