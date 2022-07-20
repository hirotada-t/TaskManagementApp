<template>
  <q-card class="my-card q-mb-md">
    <q-card-section>
      {{taskTitle}}
    </q-card-section>
  </q-card>
</template>

<script>
  export default {
    name: 'TaskItem',
    props: {
      msg: String
    },
    data() {
      return {
        taskTitle: "title"
      }
    }
  }
</script>

<style lang="scss" scoped>
  .my-card {
    width: 100%;
    max-width: 250px;
  }
</style>
