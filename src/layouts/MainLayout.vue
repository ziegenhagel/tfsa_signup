
<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar icon="fa fa-sign-in-alt"/>
          Filmstadt Arendsee Anmeldungen
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>


<script lang="ts">
export default {
  name: 'MainLayout',
  data: ()=>{return {
    message: 'wlle wlfd sjl',
  }},
}
</script>
