<template>
  <div class="q-pa-sm w-300px" v-if="!getSection.archives">
    <q-card class="my-card bg-blue-grey-1">
      <q-card-section class="row justify-around section-name order-xs-first">
        <q-input borderless v-model="getSection.sectionName" class="text-h6" placeholder="sectionName" />
        <q-btn flat padding="xs" icon="more_vert">
          <q-menu transition-show="scale" transition-hide="scale">
            <q-list style="min-width: 100px">
              <q-item clickable @click="archiveSection">
                <q-item-section>archive section</q-item-section>
              </q-item>
              <q-separator />
              <q-item clickable @click="sortPriority">
                <q-item-section>sort priority</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
      </q-card-section>
      <q-card-section class="h-full q-py-none">
        <div v-for="card of getSection.cardList" :key="card.cardPosNum">
          <TaskItem :card="card"></TaskItem>
        </div>
        <q-card class="my-card q-mb-md" v-if="newCardInput">
          <q-card-section class="q-py-none q-px-sm card-name text-h6">
            <q-input borderless class="new-card" v-model="newCard" placeholder="cardName" />
            <q-btn label="add" color="primary" @click="addCard" class="q-my-sm" />
          </q-card-section>
        </q-card>
      </q-card-section>
      <q-btn flat class="full-width q-py-sm bg-dark text-white" label="＋ add card" @click="addSectionInput" />
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
        newCard: "",
        newCardInput: false,
      }
    },

    methods: {
      addSectionInput() {
        this.newCardInput = !this.newCardInput;
        this.$nextTick(() => {
          if (document.querySelector(".new-card input") !== null) {
            document.querySelector(".new-card input").focus();
          }
        });
      },
      addCard() {
        this.newCardInput = !this.newCardInput;
        const date = new Date();
        this.getSection.cardList.push({
          "cardName": this.newCard ? this.newCard : "No card title",
          // "cardPosNum": this.getSection.cardList.length + 1,
          "cardContent": "content",
          // "createDate": date.toLocaleString(),
          // "deadLine": "",
          // "checkList": {},
          // "cardTags": [],
          "priority": "",
          "checked": false,
          "archives": false,
          // "cardComment": "comment",
        });
        this.newCard = ""
      },
      archiveSection() {
        this.$q.dialog({
          title: 'Alert',
          message: 'All cards in the section are also archived. Are you sure??',
          cancel: true
        }).onOk(() => {
          this.getSection.archives = true;
          for (let i = 0; i < this.getSection.cardList.length; i++) {
            this.getSection.cardList[i].archives = true;
          }
        });
      },
    },
    computed: {
    },

    mounted() {
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
