<template>
  <q-page>
    <div class="column" style="height: calc(100vh - 100px);">
      <p class="text-center text-h3 q-mt-auto">Tap the image to get started.</p>
      <div class="q-mt-md q-mb-auto text-center">
        <img src="../img/lets-start.png" alt="" width="300" @click="startConfig = true">
      </div>
    </div>
    <q-dialog v-model="startConfig">
      <q-card>
        <q-card-section class="row justify-between q-pb-none">
          <div class="text-h5">How do you start?</div>
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>
        <q-card-section>
          <q-btn label="start anew" icon="open_in_new" size="20px" color="primary" class="q-mt-md full-width"
            to="/task" />
          <q-btn label="open json data" icon="file_upload" size="20px" color="primary" class="q-mt-md full-width"
            @click="btnclick" />
          <input type="file" class="hidden" ref="input" accept="application/json" @change="fileHandle">
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>

  export default {

    data() {
      return {
        startConfig: false,
      }
    },
    methods: {
      btnclick() {
        this.$refs.input.click();
      },
      async fileHandle() {
        const file = this.$refs.input.files[0];
        if (!file) {
          return;
        }
        let reader = new FileReader();
        reader.readAsText(file);
        reader.onload = () => {
          this.$router.push({
            name: "TaskPage",
            params: { taskList: JSON.parse(reader.result) }
          });
        };
      },
    }
  }
</script>
