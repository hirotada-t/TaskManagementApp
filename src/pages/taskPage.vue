<template>
  <q-page>
    <q-header elevated class="bg-grey-7 desktop-only text-black">
      <q-toolbar class="q-py-sm q-gutter-md">
        <div class="col-1">
          <q-btn icon="keyboard_return" class="full-width bg-grey-4" label="back" size="15px" href="/" />
        </div>
        <div class="col-3 task-list-title">
          <q-input dense outlined bg-color="grey-2" v-model="taskListTitle" placeholder="taskListTitle"
            class="full-width" style="font-size: 25px;" />
        </div>
        <div class="col-2">
          <q-btn label="save" class="full-width bg-deep-orange-3" @click="saveData" size="15px" icon="save_alt" />
        </div>
        <div class="col-2">
          <q-btn :label="filtered ? 'filtered' : 'unfiltered'" class="full-width bg-light-blue-3"
            @click="filtered = !filtered" size="15px" :icon="rmvUncleared">
            <q-tooltip>
              Remove uncleared
            </q-tooltip>
          </q-btn>
        </div>
        <q-btn dense flat class="text-white q-ml-auto" round icon="menu" @click="rightDrawerOpen = !rightDrawerOpen" />
      </q-toolbar>
    </q-header>

    <q-drawer v-model="rightDrawerOpen" side="right" overlay behavior="mobile" bordered>
      <div class="column menu-justify bg-blue-grey-1" style="height:100%;">
        <div class="desktop-only">
          <div class="text-right q-px-md">
            <q-btn flat round icon="close" label="close" @click="rightDrawerOpen = !rightDrawerOpen" />
          </div>
          <q-list bordered class="rounded-borders">
            <q-expansion-item class="text-h5" expand-separator label="Others">
              <q-list class="text-h6">
                <q-item clickable v-ripple href="/#/task" target="_brank">
                  <q-item-section class="items-end" avatar>
                    <q-icon name="open_in_new" />
                  </q-item-section>
                  <q-item-section>create anew</q-item-section>
                </q-item>
                <q-item clickable v-ripple href="/">
                  <q-item-section class="items-end" avatar>
                    <q-icon name="keyboard_return" />
                  </q-item-section>
                  <q-item-section>back to Top</q-item-section>
                </q-item>
              </q-list>
            </q-expansion-item>
          </q-list>
        </div>
        <p class="text-h5 q-pt-md q-pl-md">Archive List</p>
        <div v-if="archiveList.length != 0">
          <div class="archive-area bg-blue-grey">
            <div v-for="item of archiveList" :key="item.cardId">
              <ArchiveItem :archiveItem="item"></ArchiveItem>
            </div>
          </div>
        </div>
        <div v-else class="text-center">
          <p class="text-h6 text-indigo-7"><span class="material-icons">search_off</span>No Archives…</p>
        </div>
        <div class="mobile-only">
          <q-list bordered class="rounded-borders">
            <q-expansion-item expand-separator icon="menu" label="other menu">
              <q-card>
                <q-card-section>aaaa</q-card-section>
              </q-card>
            </q-expansion-item>
          </q-list>
          <div class="text-right q-px-md">
            <q-btn flat round icon="close" label="close" @click="rightDrawerOpen = !rightDrawerOpen" />
          </div>
        </div>
      </div>
    </q-drawer>

    <div class="task-container">
      <div class="q-py-md q-py-md viewport">
        <div class="row no-wrap q-gutter-none content">
          <div v-for="section of getTaskList" :key="section.sectionId">
            <TaskColumn :section="section" :filter="filtered" @add-archive-list="addArchiveList"></TaskColumn>
          </div>
          <div class="q-pa-sm w-300px" v-if="newSectionInput">
            <q-card class="my-card bg-blue-grey-1 q-pa-md">
              <q-input borderless class="new-section" v-model="newSection" placeholder="sectionName" />
              <q-btn label="add" color="primary" @click="addSection" class="q-mt-sm" />
            </q-card>
          </div>
          <div class="q-pa-sm w-300px">
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
      <q-tabs align="justify">
        <q-tab dense flat round icon="keyboard_return" href="/" />
        <q-tab dense flat round icon="save_alt" @click="saveData" />
        <q-tab dense flat round :icon="scaleIcon" @click="scaleCardArea" />
        <q-tab dense flat round :icon="rmvUncleared" @click="filtered = !filtered" />
        <q-tab dense flat round icon="menu" @click="rightDrawerOpen = !rightDrawerOpen" />
      </q-tabs>
    </q-footer>

  </q-page>
</template>

<script>
  import TaskColumn from '../components/TaskColumn.vue';
  import ArchiveItem from '../components/ArchiveItem.vue';
  import ScrollBooster from 'scrollbooster';
  import { createApp, nextTick } from 'vue'

  export default {
    name: 'PageIndex',

    props: {
      taskList: { type: Array }
    },

    components: {
      TaskColumn,
      ArchiveItem,
    },

    data() {
      return {
        scaleIcon: "zoom_in",
        rightDrawerOpen: false,
        getTaskList: this.taskList ? this.taskList : [],
        scrollBooster: null,
        taskListTitle: "",
        newSection: "",
        newSectionInput: false,
        filtered: false,
        archiveList: [],
      }
    },

    methods: {
      addSectionInput() {
        this.newSectionInput = !this.newSectionInput;
        this.$nextTick(() => {
          const newSection = document.querySelector(".new-section input");
          newSection.focus();
          newSection.addEventListener("keydown", (e) => {
            if (e.key === "Enter") {
              this.addSection();
            }
          })
        });
        if (window.matchMedia && window.matchMedia('(min-device-width: 1024px)').matches) {
          this.updateScrollBooster();
        }
      },
      addSection() {
        const date = new Date();
        this.newSectionInput = !this.newSectionInput;
        this.getTaskList.push({
          "sectionId": "s-" + date.toLocaleString(),
          "sectionName": this.newSection ? this.newSection : "No section title",
          "sectionPosNum": this.getTaskList.length + 1,
          "archives": false,
          "cardList": []
        });
        this.newSection = "";
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
          alert("保存をキャンセルしました。");
        }
      },
      confirmSave(event) {
        event.returnValue = "";
      },
      scaleCardArea() {
        const content = document.querySelector(".content");
        if (content.classList.contains("zoom-out")) {
          content.classList.remove("zoom-out");
          this.scaleIcon = "zoom_out";
        } else {
          content.classList.add("zoom-out");
          content.style.width = window.screen.width + "px";
          this.scaleIcon = "zoom_in";
        }
      },
      addArchiveList(e) {
        this.archiveList.unshift(e);
      },
    },

    computed: {
      rmvUncleared() {
        if (this.filtered) return "filter_alt";
        else return "filter_alt_off";
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
      if (window.matchMedia && window.matchMedia('(min-device-width: 1024px)').matches) {
        this.scrollBooster = this.setScrollBooster(viewport, content);
      }

      const taskListTitle = document.querySelector(".task-list-title");
      taskListTitle.addEventListener("keydown", (e) => {
        if (e.key === "Enter") {
          this.saveData();
        }
      });

      if (typeof this.taskList === "undefined") return;
      if (this.taskList.length > 0) {
        for (let i = 0; i < this.taskList.length; i++) {

          if (this.taskList[i].cardList.length > 0) {
            for (let j = 0; j < this.taskList[i].cardList.length; j++) {
              if (this.taskList[i].cardList[j].archives) {
                this.archiveList.push({
                  "cardName": this.taskList[i].cardList[j].cardName,
                  // "cardPosNum": this.getSection.cardList.length + 1,
                  // "cardContent": "content",
                  // "createDate": date.toLocaleString(),
                  // "deadLine": "",
                  // "checkList": {},
                  // "cardTags": [],
                  "priority": this.taskList[i].cardList[j].priority,
                  "checked": this.taskList[i].cardList[j].checked,
                  // "cardComment": "comment",
                });
              }
            }
          }

        }
      }
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
  .menu-justify {
    @media screen and (max-width:1023px) {
      justify-content: space-between;
    }
  }

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
    transform-origin: left top;
    transform: scale(0.5);
    margin-top: auto;
    height: 200%;
  }

  .archive-area {
    max-height: calc(100vh - 280px);
    overflow: auto;
  }

  .archive-area::-webkit-scrollbar {
    width: 5px;
  }

  .archive-area::-webkit-scrollbar-track {
    background-color: #55555570;
    border-radius: 100px;
    margin: 8px;
  }

  .archive-area::-webkit-scrollbar-thumb {
    border-radius: 100px;
    background-color: #eee;
  }
</style>
