<template>
  <q-card class="my-card q-mb-md">
    <q-card-section class="q-py-none q-px-sm">
      <q-input borderless v-model="getCards.cardName" label="cardName"/>
    </q-card-section>
  </q-card>
</template>

<script>
  export default {
    name: 'TaskItem',

    props: {
      card: Object
    },

    data() {
      return {
        getCards: this.card
      }
    },

    computed: {
    },
  }
</script>

<style lang="scss" scoped>
  .my-card {
    width: 100%;
    max-width: 250px;
  }
</style>
