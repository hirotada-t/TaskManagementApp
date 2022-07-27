<template>
  <q-page>
    <q-header elevated class="bg-grey-7 desktop-only text-black">
      <q-toolbar class="row no-wrap item-center q-px-md q-py-sm q-gutter-md">
        <div>
          <q-btn icon="keyboard_return" class="full-width bg-grey-4" label="back" size="15px" href="/" />
        </div>
        <div class="flex item-centerb task-list-title">
          <q-input dense outlined bg-color="grey-2" v-model="taskListTitle" placeholder="taskListTitle"
            class="full-width" style="font-size: 25px;" />
        </div>
        <div class="col-2">
          <q-btn label="save" class="full-width bg-deep-orange-3" @click="saveData" size="15px" icon="save_alt" />
        </div>
      </q-toolbar>
    </q-header>

    <div class="task-container">
      <div class="q-py-md q-py-md viewport">
        <div class="row no-wrap q-gutter-none content">
          <div v-for="section of getTaskList" :key="section.sectionPosNum">
            <TaskColumn :section="section"></TaskColumn>
          </div>
          <div class="q-pa-sm w-300px" v-if="newSectionInput">
            <q-card class="my-card bg-blue-grey-1 q-pa-md">
              <q-input borderless class="new-section" v-model="newSection" placeholder="sectionName" />
              <q-btn label="add" color="primary" @click="addSection" class="q-mt-sm" />
            </q-card>
          </div>
          <div class="q-pa-sm w-300px \">
            <q-card class="my-card bg-blue-grey-1">
              <q-btn flat class="full-width" label="＋ add sectiion" @click="addSectionInput" />
            </q-card>
          </div>
        </div>
      </div>
      <q-input dense outlined bg-color="grey-2" v-model="taskListTitle" placeholder="taskListTitle"
        class="full-width mobile-only" style="font-size: 20px;" />
    </div>

    <q-footer elevated class="bg-blue-grey-9 text-white mobile-only">
      <q-tabs align="center">
        <q-tab dense flat round icon="keyboard_return" />
        <q-tab dense flat round icon="save_alt" @click="saveData" />
        <q-tab dense flat round :icon="scaleIcon" @click="scaleCardArea" />
      </q-tabs>
    </q-footer>
  </q-page>
</template>

<script>
  import TaskColumn from '../components/TaskColumn.vue';
  import ScrollBooster from 'scrollbooster';
  import { createApp, nextTick } from 'vue'

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
        scaleIcon: "zoom_in",
        getTaskList: this.taskList ? this.taskList : [],
        scrollBooster: null,
        taskListTitle: "",
        newSection: "",
        newSectionInput: false,
      }
    },

    methods: {
      addSectionInput() {
        this.newSectionInput = !this.newSectionInput;
        this.$nextTick(() => {
          if (document.querySelector(".new-section input") !== null) {
            document.querySelector(".new-section input").focus();
          }
        });
        this.updateScrollBooster();
      },
      addSection() {
        this.newSectionInput = !this.newSectionInput;
        this.getTaskList.push({
          "sectionName": this.newSection ? this.newSection : "No section title",
          "sectionPosNum": this.getTaskList.length + 1,
          "archives": false,
          "cardList": []
        });
        this.newSection = ""
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
          suggestedName: this.taskListTitle ? this.taskListTitle : date.toLocaleString(),
          types: [{
            description: 'JSON file',
            accept: { 'application/json': ['.json'] },
          }],
        };
        try {
          const handle = await window.showSaveFilePicker(options);
          const writable = await handle.createWritable();
          await writable.write(JSON.stringify(this.getTaskList));
          await writable.close();
        } catch (e) {
          alert("保存をキャンセルしました。")
        }
      },
      confirmSave(event) {
        event.returnValue = "";
      },
      scaleCardArea() {
        const content = document.querySelector(".content");
        if (content.classList.contains("zoom-out")) {
          content.classList.remove("zoom-out");
          this.scaleIcon = "zoom_in";
        } else {
          content.classList.add("zoom-out");
          this.scaleIcon = "zoom_out";
        }
      }
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
      // viewport.addEventListener("click", (e) => {
      //   if (e.target !== document.activeElement) {
      //     document.activeElement.blur();
      //   }
      // });

      const taskListTitle = document.querySelector(".task-list-title");
      taskListTitle.addEventListener("keydown", (e) => {
        if (e.key === "Enter") {
          this.saveData();
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
      height: calc(100vh - 140px);
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

    @media screen and (max-width:599px) {
      display: flex;
      align-items: flex-end;
    }
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

    @media screen and (max-width:599px) {
      align-items: flex-end;
    }
  }

  .zoom-out {
    @media screen and (max-width:1023px) {
      transform-origin: left top;
      transform: scale(0.5);
    }
  }
</style>
