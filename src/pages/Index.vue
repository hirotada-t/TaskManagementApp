<template>
  <q-page>
    <div class="q-py-md task-container">
      <transition name="scale">
        <div class="viewport">
          <div class="row no-wrap q-gutter-none content scale-on">
            <TaskColumn cards="2"></TaskColumn>
            <TaskColumn cards="15"></TaskColumn>
            <TaskColumn cards="1"></TaskColumn>
            <TaskColumn cards="1"></TaskColumn>
            <TaskColumn cards="4"></TaskColumn>
            <TaskColumn cards="4"></TaskColumn>
            <TaskColumn cards="4"></TaskColumn>
            <div class="q-pa-sm w-300px">
              <q-card class="my-card bg-blue-grey-1">
                <q-btn flat class="full-width" label="＋ add sectiion" @click="addSection" />
              </q-card>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </q-page>
</template>

<script>
  import TaskColumn from '../components/TaskColumn.vue'
  import ScrollBooster from 'scrollbooster';

  // Horizontal Image Slider
  window.onload = function () {
    new ScrollBooster({
      viewport: document.querySelector(".viewport"),
      content: document.querySelector(".content"),
      direction: "horizontal",
      scrollMode: "native",
    });
  }

  export default {
    name: 'PageIndex',
    components: {
      TaskColumn,
    },
    methods: {
      addSection() {
        alert("section")
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
    transition:.3s;
  }

  .scale-on {
    transform-origin:left top;
    transform: scale(0.5);
  }
</style>
