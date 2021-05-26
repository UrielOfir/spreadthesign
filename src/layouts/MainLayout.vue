<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated style="background-color: #26a69a">
      <q-toolbar>
        <q-toolbar-title style="color: #ffffff">
          Spread The Sign
        </q-toolbar-title>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',

  data () {
    return {
      leftDrawerOpen: false,

    }
  }
}
</script>
 <style>



 </style>
