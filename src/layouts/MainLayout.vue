<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="bg-header text-white">
      <q-toolbar>
        <q-toolbar-title>
          Task Management App
        </q-toolbar-title>
        <q-btn dense flat round icon="menu" @click="right = !right" class="desktop-only"/>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="right" side="right" overlay behavior="mobile" elevated>
      <div class="column" style="height: 100%;">
        <q-list>
          <q-item-label header class="text-grey-8">
            Essential Links
          </q-item-label>
          <EssentialLink v-for="link in essentialLinks" :key="link.title" v-bind="link" />
        </q-list>
        <div class="q-mb-lg text-center" style="margin-top: auto;">
          <q-btn flat class="bg-grey-5" icon="close" label="close" @click="right = !right" />
        </div>
      </div>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>


    <q-footer elevated class="bg-blue-grey-9 text-white mobile-only">
      <q-tabs align="center">
        <q-tab dense flat round icon="close" />
        <q-tab dense flat round :icon="scaleIcon" @click="scaleCardArea"/>
        <q-tab dense flat round icon="menu" @click="right = !right" />
      </q-tabs>
    </q-footer>

  </q-layout>
</template>

<script>
  import EssentialLink from 'components/EssentialLink.vue'

  const linksData = [
    {
      title: 'Docs',
      caption: 'quasar.dev',
      icon: 'school',
      link: 'https://quasar.dev'
    },
    {
      title: 'Github',
      caption: 'github.com/quasarframework',
      icon: 'code',
      link: 'https://github.com/quasarframework'
    },
    {
      title: 'Discord Chat Channel',
      caption: 'chat.quasar.dev',
      icon: 'chat',
      link: 'https://chat.quasar.dev'
    },
    {
      title: 'Forum',
      caption: 'forum.quasar.dev',
      icon: 'record_voice_over',
      link: 'https://forum.quasar.dev'
    },
    {
      title: 'Twitter',
      caption: '@quasarframework',
      icon: 'rss_feed',
      link: 'https://twitter.quasar.dev'
    },
    {
      title: 'Facebook',
      caption: '@QuasarFramework',
      icon: 'public',
      link: 'https://facebook.quasar.dev'
    },
    {
      title: 'Quasar Awesome',
      caption: 'Community Quasar projects',
      icon: 'favorite',
      link: 'https://awesome.quasar.dev'
    }
  ]

  export default {
    name: 'MainLayout',
    components: {
      EssentialLink
    },
    data() {
      return {
        right: false,
        essentialLinks: linksData,
        scaleIcon:"zoom_in"
      }
    },
    methods: {
      scaleCardArea(){
        const content = document.querySelector(".content");
        if(content.classList.contains("zoom-out")){
          content.classList.remove("zoom-out");
          this.scaleIcon = "zoom_in";
        } else {
          content.classList.add("zoom-out");
          this.scaleIcon = "zoom_out";
        }
      }
    }
  }
</script>

<style>
  .bg-header {
    background-color: #00000099;
  }
</style>
