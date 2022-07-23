<template>
  <div class="q-pa-sm w-300px">
    <q-card class="my-card bg-blue-grey-1">
      <q-card-section>
        <div class="text-h6">{{getSection.sectionName}}</div>
      </q-card-section>
      <q-card-section class="h-full q-py-none">
        <div v-for="card of getSection.cardList" :key="card.cardPosNum">
          <TaskItem :card="card"></TaskItem>
        </div>
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
      section: { type: Object }
    },

    components: {
      TaskItem,
    },

    data() {
      return {
        getSection: this.section,
      }
    },

    methods: {
      addCard() {
        this.getSection.cardList.push({
          "cardName": "newCard",
          "cardPosNum": this.getSection.cardList.length + 1,
          "cardContent": "content",
          "createDate": "2022-07-22",
          "deadLine": "",
          "checkList": {},
          "cardTags": [],
          "archives": false,
          "cardComment": "comment",
          "cardStar": false
        })
      }
    },

    computed: {
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
