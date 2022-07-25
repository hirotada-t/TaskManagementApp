<template>
  <div class="q-pa-sm w-300px">
    <q-card class="my-card bg-blue-grey-1">
      <q-card-section class="section-name order-xs-first">
        <q-input borderless v-model="getSection.sectionName" class="text-h6" label="sectionName" />
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
  import TaskItem from './TaskItem.vue';

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
        const date = new Date();
        this.getSection.cardList.push({
          "cardName": "",
          "cardPosNum": this.getSection.cardList.length + 1,
          "cardContent": "content",
          "createDate": date.toLocaleString(),
          "deadLine": "",
          "checkList": {},
          "cardTags": [],
          "archives": false,
          "cardComment": "comment",
          "cardStar": false
        });
      },
    },

    computed: {
    },

    mounted() {
      // get out of focus
      const sectionName = document.querySelector(".section-name input");
      sectionName.addEventListener("keydown", (e) => {
        if (e.key === "Enter") {
          document.activeElement.blur();
        }
      });
    },
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
    max-height: calc(100vh - 290px);
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
