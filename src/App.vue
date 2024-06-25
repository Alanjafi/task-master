<template>
  <v-app>
    <v-main>
      <KanbanBoard />
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
import { createVuetify } from 'vuetify'
import 'vuetify/styles'
import { aliases, mdi } from 'vuetify/iconsets/mdi'
import KanbanBoard from './components/KanbanBoard.vue'

const vuetify = createVuetify({
  icons: {
    defaultSet: 'mdi',
    aliases,
    sets: {
      mdi,
    },
  },
})

</script>

<style>
@import 'vuetify/dist/vuetify.css';
</style>
