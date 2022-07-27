<template>
  <q-card class="my-card q-mb-md" v-if="!getCards.archives" :class="getCards.priority">
    <q-card-section class="q-py-none q-px-sm card-name">
      <span class="text-h6">{{getCards.cardName}}</span>
    </q-card-section>
    <q-card-section class="row justify-start items-center q-py-none q-px-sm card-name">
      <div class="col-2">
        <q-checkbox v-model="getCards.checked" color="black" />
        <q-tooltip v-if="getCards.checked">
          Cleared!
        </q-tooltip>
        <q-tooltip v-if="!getCards.checked">
          Not cleared
        </q-tooltip>
      </div>
      <div class="col-3">
        <q-btn flat @click="getCards.archives = true" icon="archive" />
        <q-tooltip>
          Archive
        </q-tooltip>
      </div>
      <div class="col-6">
        <q-select v-model="getCards.priority" :options="options" label="priority" />
      </div>
    </q-card-section>
  </q-card>
</template>

<script>
  export default {
    name: 'TaskItem',

    props: {
      card: Object
    },

    data() {
      return {
        getCards: this.card,
        options: ["none", "high", "middle", "low"],
      }
    },

    methods: {
    },

    computed: {
    },

    mounted() {
    },
  }
</script>

<style lang="scss" scoped>
  .my-card {
    width: 100%;
    max-width: 250px;
  }

  .high {
    background-color: #ff7f7f;
  }

  .middle {
    background-color: #ffff7f;
  }

  .low {
    background-color: #7fffbf;
  }
</style>
