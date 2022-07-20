<template>
  <div class="q-pa-sm w-300px">
    <q-card class="my-card bg-blue-grey-1">
      <q-card-section>
        <div class="text-h6">{{sectionTitle}}</div>
      </q-card-section>
      <q-card-section>
        <TaskItem v-for="n of getCardsNum" :key="n"></TaskItem>
        <q-btn flat class="full-width" label="＋ add card" @click="addCard" />
      </q-card-section>
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
</style>
