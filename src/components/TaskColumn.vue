<template>
  <div class="q-pa-sm w-300px">
    <q-card class="my-card bg-blue-grey-1">
      <q-card-section>
        <div class="text-h6">{{sectionTitle}}</div>
      </q-card-section>
      <q-card-section class="h-full q-py-none">
        <TaskItem v-for="n of getCardsNum" :key="n"></TaskItem>
      </q-card-section>
      <q-btn flat class="full-width q-py-sm bg-dark text-white" label="＋ add card" @click="addCard" />

    </q-card>
  </div>
</template>

<script>
  import TaskItem from './TaskItem.vue'

  export default {
    name: 'TaskColumn',
    props: {
      cards: { type: String }
    },
    components: {
      TaskItem,
    },
    data() {
      return {
        sectionTitle: "section",
        getCardsNum: Number(this.cards),
      }
    },
    methods: {
      addCard() {
        alert("card")
      }
    }
  }
</script>

<style lang="scss" scoped>
  .w-300px {
    min-width: 300px;
  }

  .h-full {
    @media screen and (min-width:1024px) {
      max-height: calc(100vh - 260px);
    }

    margin-right: 5px;
    max-height: calc(100vh - 300px);
    overflow-x: auto;
  }

  .h-full::-webkit-scrollbar {
    width: 7px;
  }

  .h-full::-webkit-scrollbar-track {
    background-color: #55555540;
    border-radius: 100px;
    margin-bottom: 10px;
  }

  .h-full::-webkit-scrollbar-thumb {
    border-radius: 100px;
    background-color: #55555580;
  }

</style>
