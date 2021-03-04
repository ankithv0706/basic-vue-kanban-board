<template>
  <div class="home">
    <div class="board container">
      <div class="row">
        <div v-for="tab in tabKeys" :key="tab">
          <div class="col-md-3">
            <h1>{{ tab }}</h1>
            <draggable v-model="$data[tab]" group="cards">
              <task-card
                :card="card"
                v-for="(card, cKey) in $data[tab]"
                :key="cKey"
                @click.native="openModal(card, cKey, tab)"
              />
            </draggable>
            <div @click="addTask(tab)">Add Task</div>
          </div>
        </div>
      </div>
      <card-modal
        :card="selectedCard"
        v-if="viewModal"
        @close="closeModal"
        @save="saveCard"
      />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import TaskCard from "@/components/TaskCard.vue";
import CardModal from "@/components/CardModal.vue";
import Draggable from "vuedraggable";
export default {
  name: "home",
  data() {
    return {
      drag: false,
      viewModal: false,
      selectedCard: {},
      curruntKey: 0,
      curruntTab: "",
      tasks: [],
      completed: [],
      deferred: [],
      rejected: [],
      tabKeys: ["tasks", "completed", "deferred", "rejected"],
    };
  },
  components: {
    TaskCard,
    CardModal,
    Draggable,
  },
  mounted() {
    this.getLocalStorage();
  },
  methods: {
    addTask(dataPoint) {
      this[dataPoint].push({ title: "Title", description: "", color: "blue" });
      // this.setLocalStorage();
    },
    openModal(card, ckey, tab) {
      this.selectedCard = card;
      this.curruntKey = ckey;
      this.curruntTab = tab;
      this.viewModal = true;
    },
    saveCard(cardDetail) {
      this[this.curruntTab][this.curruntKey] = cardDetail;
      this.selectedCard = {};
      this.curruntKey = 0;
      this.curruntTab = "";
      this.viewModal = false;
      this.setLocalStorage();
    },
    closeModal() {
      this.viewModal = false;
      this.setLocalStorage();
    },
    setLocalStorage() {
      this.tabKeys.forEach((key) => {
        localStorage.setItem(key, JSON.stringify(this[key]));
      });
    },
    getLocalStorage() {
      this.tabKeys.forEach((key) => {
        this[key] = localStorage.getItem(key)
          ? JSON.parse(localStorage.getItem(key))
          : [];
      });
    },
  },
};
</script>
