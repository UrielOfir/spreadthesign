<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated style="background-color: #0c0c0c">
      <q-toolbar>
        <q-toolbar-title style="color: #ffffff">
          Spread The Sign
        </q-toolbar-title>
        <img src="../sign_images/symbol.png" style="width:50px">
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
