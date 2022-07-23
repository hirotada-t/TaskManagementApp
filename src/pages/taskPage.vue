<template>
  <q-page>
    <div class="q-py-md task-container">
      <div class="viewport">
        <div class="row no-wrap q-gutter-none content">
          <div v-for="section of getTaskList" :key="section.sectionPosNum">
            <TaskColumn :section="section"></TaskColumn>
          </div>
          <div class="q-pa-sm w-300px">
            <q-card class="my-card bg-blue-grey-1">
              <q-btn flat class="full-width" label="＋ add sectiion" @click="addSection" />
            </q-card>
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
  import TaskColumn from '../components/TaskColumn.vue';
  import ScrollBooster from 'scrollbooster';

  export default {
    name: 'PageIndex',

    props: {
      taskList: { type: Array }
    },

    components: {
      TaskColumn,
    },

    data() {
      return {
        getTaskList: this.taskList,
        scrollBooster: null,
      }
    },

    methods: {
      addSection() {
        this.getTaskList.push({
          "sectionName": "newSection",
          "sectionPosNum": this.getTaskList.length + 1,
          "cardList": []
        });
        this.updateScrollBooster();
      },
      setScrollBooster(viewport, content) {
        return new ScrollBooster({
          viewport,
          content,
          direction: "horizontal",
          scrollMode: "native",
        });
      },
      updateScrollBooster() {
        this.$nextTick(() => {
          this.scrollBooster.updateMetrics();
        });
      },
    },

    computed: {
    },

    mounted() {
      // Horizontal Image Slider
      let viewport = document.querySelector(".viewport");
      let content = document.querySelector(".content");
      this.scrollBooster = this.setScrollBooster(viewport, content);
    }
  }
</script>

<style lang="scss" scoped>
  .task-container {
    @media screen and (min-width:1024px) {
      height: calc(100vh - 50px);
    }

    height: calc(100vh - 100px);
  }

  .w-300px {
    min-width: 300px;
  }

  .viewport {
    overflow-x: auto;
    overflow-y: hidden;
    height: 100%;
    margin: auto;
  }

  .viewport::-webkit-scrollbar {
    height: 10px;
  }

  .viewport::-webkit-scrollbar-track {
    background-color: #55555570;
    border-radius: 100px;
    margin: 10px;
  }

  .viewport::-webkit-scrollbar-thumb {
    border-radius: 100px;
    background-color: #eee;
  }

  .content {
    transition: .3s;
  }

  .zoom-out {
    transform-origin: left top;
    transform: scale(0.5);
  }
</style>
