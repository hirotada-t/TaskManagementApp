<template>
  <q-card class="gnavi my-card q-mb-md" v-if="!getCards.archives" :class="getCards.priority">
    <div class="cleared-border" :class="cleared">
      <div>
        <q-card-section class="q-py-none q-px-sm card-name">
          <q-input borderless autogrow v-model="getCards.cardName" class="col-11 text-h6" placeholder="cardName" />
          <!-- <span class="text-h6" @click="setDetails = true">{{getCards.cardName}}</span> -->
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
            <q-btn flat @click="archiveCard" icon="archive" />
            <q-tooltip>
              Archive
            </q-tooltip>
          </div>
          <div class="col-6 q-py-sm">
            <q-select dense filled borderless v-model="getCards.priority" :options="options" label="priority" />
          </div>
        </q-card-section>
      </div>
    </div>
    <!-- <q-dialog v-model="setDetails">
      <q-layout view="hhh lpr fff" container class="bg-white">
        <q-header class="bg-blue-grey sp-none">
          <q-toolbar>
            <q-toolbar-title>
              <q-input dense outlined bg-color="grey-4" v-model="getCards.cardName" label="change card title"
                class="full-width" style="font-size: 25px;" />
            </q-toolbar-title>
            <q-btn dense flat round icon="menu" @click="rightDrawerOpen = !rightDrawerOpen" />
            <q-btn flat v-close-popup round dense icon="close" />
          </q-toolbar>
        </q-header>
        <q-footer class="bg-blue-grey">
          <q-toolbar class="tb-pc-none">
            <q-toolbar-title>
              <q-input dense outlined bg-color="grey-4" v-model="getCards.cardName" label="change card title"
                class="full-width" style="font-size: 25px;" />
            </q-toolbar-title>
            <q-btn flat v-close-popup round dense icon="close" />
          </q-toolbar>
        </q-footer>

        <q-page-container>
          <q-page padding>
            <p>
              <q-expansion-item v-model="expanded" icon="perm_identity" label="Description">
                <q-card>
                  <q-card-section>
                    Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quidem, eius reprehenderit eos corrupti
                    commodi magni quaerat ex numquam, dolorum officiis modi facere maiores architecto suscipit iste
                    eveniet doloribus ullam aliquid.
                  </q-card-section>
                </q-card>
              </q-expansion-item>
            </p>
          </q-page>
        </q-page-container>
      </q-layout>
    </q-dialog> -->
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
        drawerR: false,
        setDetails: false,
        expanded: true,
      }
    },

    methods: {
      archiveCard() {
        let message = "";
        if (!this.getCards.checked) {
          message = "The task is uncompleted. Do you archive?"
        } else {
          message = "Do you archive the task?"
        }
        this.$q.dialog({
          title: 'Alert',
          message,
          cancel: {
            push: true,
            color: 'negative'
          }
        }).onOk(() => {
          this.getCards.archives = true;
        });
      },
    },

    computed: {
      cleared() {
        if (this.getCards.checked) return "cleared";
        return "";
      },
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

  .gnavi .cleared-border {
    position: relative;
  }

  .gnavi .cleared-border::before,
  .gnavi .cleared-border::after {
    content: "";
    position: absolute;
    width: 0;
    height: 3px;
    background-color: #fc693b;
    background-image:
      repeating-linear-gradient(-45deg,
        #fff, #fff 7px,
        transparent 0, transparent 14px);
    transition: all 0.2s linear;
    transition-delay: 0.2s;
  }

  .gnavi .cleared-border::before {
    right: 0;
    top: 0;
  }

  .gnavi .cleared-border::after {
    left: 0;
    bottom: 0;
  }

  .gnavi .cleared-border div::before,
  .gnavi .cleared-border div::after {
    content: "";
    position: absolute;
    width: 3px;
    height: 0;
    background-color: #fc693b;
    background-image:
      repeating-linear-gradient(-45deg,
        #fff, #fff 7px,
        transparent 0, transparent 14px);
    transition: all 0.2s linear;
  }

  .gnavi .cleared-border div::before {
    left: 0;
    top: 0;
  }

  .gnavi .cleared-border div::after {
    right: 0;
    bottom: 0;
  }

  .gnavi .cleared::before,
  .gnavi .cleared::after {
    width: 100%;
  }

  .gnavi .cleared div::before,
  .gnavi .cleared div::after {
    height: 100%;
  }

  .sp-none {
    @media screen and (max-width:599px) {
      display: none;
    }
  }

  .tb-pc-none {
    @media screen and (min-width:600px) {
      display: none;
    }
  }
</style>
