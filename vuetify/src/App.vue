<script setup lang="ts">
import { ref } from 'vue'
import { useTheme } from 'vuetify'

const theme = useTheme()
const status = ref([])

function toggleTheme() {
  theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'
}

async function fetchStatus() {
  const res = await fetch('status.json')
  status.value = await res.json()
}

fetchStatus();
</script>

<template>
  <v-app class="rounded rounded-md">
    <v-app-bar title="狛荷屋 Komaniya Express">
      <template v-slot:append>
        <v-btn @click="toggleTheme" icon="mdi-theme-light-dark"></v-btn>
      </template>
    </v-app-bar>
    <v-main>
      <div class="h-100 pa-4 d-flex flex-column ga-4">
        <template v-if="status?.length">
          <ProviderCard v-for="info of status" :provider-info="info" />
        </template>
        <NoData v-else class="my-auto" />
      </div>
    </v-main>
  </v-app>
</template>
