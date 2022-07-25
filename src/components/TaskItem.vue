<template>
  <q-card class="my-card q-mb-md">
    <q-card-section class="q-py-none q-px-sm card-name">
      <q-input borderless v-model="getCards.cardName" label="cardName" />
    </q-card-section>
    <q-card-section class="q-py-none q-px-sm card-name">
      111
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

    methods: {
    },

    computed: {
    },

    mounted() {
      // get out of focus
      const cardName = document.querySelector(".card-name input");
      cardName.addEventListener("keydown", (e) => {
        if (e.key === "Enter") {
          document.activeElement.blur();
        }
      });
    },
  }
</script>

<style lang="scss" scoped>
  .my-card {
    width: 100%;
    max-width: 250px;
  }
</style>
