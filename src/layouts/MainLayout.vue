<template>
  <q-layout view="hHh lpR fFf" class="bg-grey-2">

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script>

  export default {
    name: 'MainLayout',

    data() {
      return {}
    },

    methods: {
    },

  }
</script>

<style>
  .bg-header {
    background-color: #00000099;
  }
</style>
