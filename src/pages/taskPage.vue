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
      <q-btn flat class="full-width bg-dark text-white q-my-xl" label="save" @click="saveData" />
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
        getTaskList: this.taskList ? this.taskList : [],
        scrollBooster: null,
      }
    },

    methods: {
      addSection() {
        this.getTaskList.push({
          "sectionName": "",
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
      async saveData() {
        const date = new Date();
        const options = {
          suggestedName: date.toLocaleString(),
          types: [{
            description: 'JSON file',
            accept: { 'application/json': ['.json'] },
          }],
        };
        const handle = await window.showSaveFilePicker(options);
        const writable = await handle.createWritable();
        await writable.write(JSON.stringify(this.getTaskList));
        await writable.close();
      },
      confirmSave(event) {
        event.returnValue = "";
      },
    },

    created() {
      window.addEventListener("beforeunload", this.confirmSave);
    },

    destroyed() {
      window.removeEventListener("beforeunload", this.confirmSave);
    },

    mounted() {
      // Horizontal Image Slider
      const viewport = document.querySelector(".viewport");
      const content = document.querySelector(".content");
      this.scrollBooster = this.setScrollBooster(viewport, content);

      // get out of focus
      const body = document.querySelector("body");
      viewport.addEventListener("click", (e) => {
        if(e.target !== document.activeElement) {
          document.activeElement.blur();
        }
      });
    },

    beforeRouteLeave(to, from, next) {
      const answer = window.confirm("保存していないデータは失われます。よろしいですか？")
      if (answer) {
        next();
      } else {
        next(false);
      }
    },
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
