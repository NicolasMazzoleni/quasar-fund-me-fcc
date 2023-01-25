<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-grey-1 text-black">
      <q-toolbar>
        <q-avatar>
          <img :src="require('../assets/metamask-logo.png')" />
        </q-avatar>
        <q-toolbar-title>
          Quasar App / Smart Contract Test / METAMASK
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'MainLayout',
});
</script>
