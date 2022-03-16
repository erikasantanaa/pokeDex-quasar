<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title class="text-center">
          <P class="q-mb-none">
            QUEM É ESSE POKEMON?
          </P>
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default ({
  name: 'MainLayout',
  data() {
    return
  }
  })
</script>
