<template>
  <q-card class="my-card q-mb-md">
    <q-card-section>
      {{getCards.cardName}}
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
    }
  }
</script>

<style lang="scss" scoped>
  .my-card {
    width: 100%;
    max-width: 250px;
  }
</style>
