<template>
  <q-page>
    <div class="q-pa-md row no-wrap q-gutter-none task-container">
      <TaskColumn cards="2"></TaskColumn>
      <TaskColumn cards="5"></TaskColumn>
      <TaskColumn cards="1"></TaskColumn>
      <TaskColumn cards="4"></TaskColumn>
      <div class="q-pa-sm w-300px">
        <q-card class="my-card bg-blue-grey-1">
          <q-btn flat class="full-width" label="＋ add sectiion" @click="addSection" />
        </q-card>
      </div>


    </div>
    <q-card v-touch-swipe.mouse.left.right="handleSwipe"
      class="custom-area cursor-pointer bg-primary text-white shadow-2 row flex-center">
      <div v-if="info" class="custom-info">
        <pre>{{ info.distance.x }}</pre>
        <pre>{{ info.direction }}</pre>
      </div>
    </q-card>
  </q-page>
</template>

<script>
  import TaskColumn from '../components/TaskColumn.vue'
  import { ref } from 'vue'

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
    setup() {
      const info = ref(null)

      return {
        info,
        handleSwipe({ evt, ...newInfo }) {
          info.value = newInfo
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .task-container {
    width: 100%;

    @media screen and (min-width:1024px) {
      height: calc(100vh - 50px);
    }

    height: calc(100vh - 110px);
    overflow-x: auto;
  }

  .w-300px {
    min-width: 300px;
  }

  .custom-area {
    width: 90%;
    height: 220px;
    border-radius: 3px;
    padding: 8px;
  }

  .custom-info pre {
    width: 180px;
    font-size: 20px;
  }
</style>
